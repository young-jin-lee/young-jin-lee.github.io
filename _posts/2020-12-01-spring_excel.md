---
layout: post
title: "Excel Download"
comments: true
categories: Java
---

**dispatcher-servlet.xml bean 추가**

- 컨트롤러에서 view로 매핑되지 않고 해당 객체로 바로 매핑하기 위해 빈즈를 등록한다. 이 때 beanNameViewResolver가 InternalResourceViewResolver가 먼저 와야한다.

```
	<resources mapping="/resources/**" location="/resources/" />

	<beans:bean id="beanNameViewResolver" class="org.springframework.web.servlet.view.BeanNameViewResolver">
        <beans:property name="order" value="0"/>
    </beans:bean>

    <!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<beans:property name="prefix" value="/WEB-INF/views" />
		<beans:property name="suffix" value=".jsp" />
		<beans:property name="order" value="1"/>
	</beans:bean>

	<beans:bean id="excelDownloadView" class="com.excel.util.ExcelDownloadView"/>
```

<hr/>

**FRONT**

- html

```
<form id="form1" name="form1" method="post" enctype="multipart/form-data">
    <input type="file" id="fileInput" name="fileInput">
    <button type="button"></button>
</form>
```

- javascript

```
let excelDownloadBtn = document.querySelector("#excelDownload");
excelDownloadBtn.onclick = function(){
        var f = document.form1;
        f.action = "downloadExcelFile";
        f.submit();
}
```

<hr/>

**Controller**

```
@RequestMapping(value = "/downloadExcelFile", method = RequestMethod.POST)
    public String downloadExcelFile_POST(Model model) throws Exception{

		List<TestVO> TestList = testService.getTestList();

        SXSSFWorkbook workbook = testService.excelFileDownloadProcess(TestList);

        model.addAttribute("locale", Locale.KOREA);
        model.addAttribute("workbook", workbook);
        model.addAttribute("workbookName", "엑셀파일명");

        return "excelDownloadView";
    }
```

<hr/>

**util class 추가**

- 엑셀 다운로드에 가장 중요한 클래스이다. 위 서블릿에서 설정한대로 excelDownloadView로 컨트롤러에서 매핑 시 해당 클래스로 매핑된다.

```
package com.test.excel.util;

import java.io.OutputStream;
import java.net.URLEncoder;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Locale;
import java.util.Map;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.poi.xssf.streaming.SXSSFWorkbook;
import org.springframework.web.servlet.view.AbstractView;

public class ExcelDownloadView extends AbstractView{

    @Override
    protected void renderMergedOutputModel(Map<String, Object> model, HttpServletRequest request, HttpServletResponse response)
            throws Exception {

        Locale locale = (Locale) model.get("locale");
        String workbookName = (String) model.get("workbookName");

        // 겹치는 파일 이름 중복을 피하기 위해 시간을 이용해서 파일 이름에 추가
        Date date = new Date();
        SimpleDateFormat dayformat = new SimpleDateFormat("yyyyMMdd", locale);
        SimpleDateFormat hourformat = new SimpleDateFormat("hhmmss", locale);
        String day = dayformat.format(date);
        String hour = hourformat.format(date);
        String fileName = workbookName + "_" + day + "_" + hour + ".xlsx";

        // 여기서부터는 각 브라우저에 따른 파일이름 인코딩작업
        String browser = request.getHeader("User-Agent");
        if (browser.indexOf("MSIE") > -1) {
            fileName = URLEncoder.encode(fileName, "UTF-8").replaceAll("\\+", "%20");
        } else if (browser.indexOf("Trident") > -1) {       // IE11
            fileName = URLEncoder.encode(fileName, "UTF-8").replaceAll("\\+", "%20");
        } else if (browser.indexOf("Firefox") > -1) {
            fileName = "\"" + new String(fileName.getBytes("UTF-8"), "8859_1") + "\"";
        } else if (browser.indexOf("Opera") > -1) {
            fileName = "\"" + new String(fileName.getBytes("UTF-8"), "8859_1") + "\"";
        } else if (browser.indexOf("Chrome") > -1) {
            StringBuffer sb = new StringBuffer();
            for (int i = 0; i < fileName.length(); i++) {
               char c = fileName.charAt(i);
               if (c > '~') {
                     sb.append(URLEncoder.encode("" + c, "UTF-8"));
                       } else {
                             sb.append(c);
                       }
                }
                fileName = sb.toString();
        } else if (browser.indexOf("Safari") > -1){
            fileName = "\"" + new String(fileName.getBytes("UTF-8"), "8859_1")+ "\"";
        } else {
             fileName = "\"" + new String(fileName.getBytes("UTF-8"), "8859_1")+ "\"";
        }

        response.setContentType("application/download;charset=utf-8");
        response.setHeader("Content-Disposition", "attachment; filename=\"" + fileName + "\";");
        response.setHeader("Content-Transfer-Encoding", "binary");

       OutputStream os = null;
       SXSSFWorkbook workbook = null;

       try {
           workbook = (SXSSFWorkbook) model.get("workbook");
           os = response.getOutputStream();

           // 파일생성
           workbook.write(os);
       }catch (Exception e) {
           e.printStackTrace();
       } finally {
           if(workbook != null) {
               try {
                   workbook.close();
               } catch (Exception e) {
                   e.printStackTrace();
               }
           }

           if(os != null) {
               try {
                   os.close();
               } catch (Exception e) {
                   e.printStackTrace();
               }
           }
       }
    }
}
```

<hr/>

**ServiceImpl 추가**

```
public SXSSFWorkbook makeExcelWorkbook(List<TestVO> list) {
        SXSSFWorkbook workbook = new SXSSFWorkbook();

        // 시트 생성
        SXSSFSheet sheet = workbook.createSheet("시트명");

        //시트 열 너비 설정
        sheet.setColumnWidth(0, 1500);
        sheet.setColumnWidth(0, 3000);
        sheet.setColumnWidth(0, 3000);
        sheet.setColumnWidth(0, 1500);

        // 헤더 행 생
        Row headerRow = sheet.createRow(0);
        // 해당 행의 첫번째 열 셀 생성
        Cell headerCell = headerRow.createCell(0);
        headerCell.setCellValue("번호");
        // 해당 행의 두번째 열 셀 생성
        headerCell = headerRow.createCell(1);
        headerCell.setCellValue("칼럼1");
        // 해당 행의 세번째 열 셀 생성
        headerCell = headerRow.createCell(2);
        headerCell.setCellValue("칼럼2");
        // 해당 행의 네번째 열 셀 생성
        headerCell = headerRow.createCell(3);
        headerCell.setCellValue("칼럼3");

        // 테이블 셀 생성
        Row bodyRow = null;
        Cell bodyCell = null;
        for(int i=0; i<list.size(); i++) {
            TestVO tvo = list.get(i);

            // 행 생성
            bodyRow = sheet.createRow(i+1);
            // 데이터 번호 표시
            bodyCell = bodyRow.createCell(0);
            bodyCell.setCellValue(i + 1);
            // 데이터 이름 표시
            bodyCell = bodyRow.createCell(1);
            bodyCell.setCellValue(tvo.getCol1());
            // 데이터 가격 표시
            bodyCell = bodyRow.createCell(2);
            bodyCell.setCellValue(tvo.getCol2());
            // 데이터 수량 표시
            bodyCell = bodyRow.createCell(3);
            bodyCell.setCellValue(tvo.getCol3());
        }

        return workbook;
    }

    public SXSSFWorkbook excelFileDownloadProcess(List<TestVO>> list) {
        return this.makeExcelWorkbook(list);
    }
```

참조: 마이자몽
