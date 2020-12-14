---
layout: post
title: "DataTables Library"
comments: true
categories: JavaScript
---

**Settings**

```
var lang_kor = {
        "decimal" : "",
        "emptyTable" : "조회된 데이터가 없습니다.",
        "info" : "총 _TOTAL_ 건",
        "infoEmpty" : "총 0건",
        "infoFiltered" : "(전체 _MAX_ 명 중 검색결과)",
        "infoPostFix" : "",
        "thousands" : ",",
        "lengthMenu" : "_MENU_ 개씩 보기",
        "loadingRecords" : "로딩중...",
        "processing" : "처리중...",
        "search" : "검색 : ",
        "zeroRecords" : "검색된 데이터가 없습니다.",
        "paginate" : {
            "first" : "첫 페이지",
            "last" : "마지막 페이지",
            "next" : "다음",
            "previous" : "이전"
        },
        "aria" : {
            "sortAscending" : " :  오름차순 정렬",
            "sortDescending" : " :  내림차순 정렬"
        }
    };
```

**Responsive**

```
let table = $('#mylist-tb').DataTable( {
	"order": [[ 5, "desc" ]],
    "columnDefs": [
        {"className": "dt-center", "targets": "_all"}
      ],
	retrieve: true,
    // 표시 건수기능
	lengthChange: false,
	// 검색 기능
	searching: false,
    responsive: true,
    language : lang_kor,
    createdRow: function(row, data, dataIndex){
        // Initialize custom control
    	yourCallbackFunction();
     }
} );
```

**CallBack on shown / hidden**

```
table.on( 'responsive-display', function ( e, datatable, row, showHide, update ) {
    console.log( 'Details for row '+row.index()+' '+(showHide ? 'shown' : 'hidden') );
    yourCallbackFunction();
});
```
