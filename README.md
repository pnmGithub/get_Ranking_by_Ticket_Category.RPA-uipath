## [일간 공연랭킹 리포트 작성]  ##
> 예스24 웹사이트에서 일간 공연 랭킹을 추출하여 결과 파일 생성      
> REFramework로 구현 (QueueItem이 아닌 **List** 이용)
>  - TransactionData :: 기존=DataTable, 변경=**List<List<String>>**
>  - TransactionItem :: 기존=QueueItem, 변경=**List<String>**   


#### [작업순서] ####
> _자동화설계_프로세스맵구상.xlsx 파일 참고
1. 예스24 티켓 페이지 접속 > 카테고리 리스트로 가져오기 (TransactionData = List<List<String>> 로 사용. <카테고리명, URL> 저장)
2. 엑셀파일 사전 생성(카테고리별 시트 생성)
3. 카테고리별 랭킹 스크래핑 (베스트랭킹+일반랭킹)
4. 카테고리별 시트에 쓰기
5. 엑셀 레이아웃 정리


#### [Config.xlsx 참고] ####
* strResultFileName - 결과파일 (티켓랭킹_yyyyMMdd.xlsx)
* strResultSaveFolder - 결과파일 저장 폴더
* strWebURL - 데이터 추출할 웹 사이트 정보 (랭킹 링크 바로가기)

  
#### [추가된 파일(Invoke) 정보] ####
* Initalization > Framework\CreateFolderFile.xaml - 폴더, 파일 처리. 폴더 없으면 생성, 파일 있으면 삭제
* Initalization > Framework\GetFullWebUrl.xaml - URL 정보 세팅 (상대 경로일 경우 도메인 붙여 full 경로 만들기)
* End Process > Framework\ExcelLayoutStyle.xaml - 엑셀파일 레이아웃 정리


#### [How It Works] ####

1. **INITIALIZE PROCESS**
   + Config.xlsx 설정
   + 브라우저(Chrome, Excel) 강제종료
   + 폴더, 파일 관리(폴더 있으면 삭제 후 생성, 파일 있으면 삭제)
   + 예스24 티켓 > 랭킹 카테고리 가져오기 (dt_TransactionData에 저장)
      1. 랭킹 카테고리 가져오기 (Find Children )
      2. -> List에 입력
   + 카테고리별 엑셀파일 사전 준비 - 시트 순서대로 작성되도록

2. **GET TRANSACTION DATA**
   + Get transaction item 비활성
   + 처리할 TransactionItem 체크

4. **PROCESS TRANSACTION**
   + 카테고리 별 NavigateTo로 랭킹 페이지 이동
   + Merge용 데이터테이블 생성 - 스크래핑 결과 없을 경우 오류 방지
   + 베스트 영역 스크래핑 ([랭킹], [공연명], [공연기간 공연장소], [예매율])
   + 랭킹 영역 스크래핑 ( [랭킹], [공연명], [공연기간 공연장소], [예매율])
   + DataTable 합치고 컬럼 정리 ([랭킹], [공연명], [공연기간], [공연장소], [예매율])
   + 카테고리별로 엑셀에 쓰기

4. **END PROCESS**
   + 엑셀 레이아웃 정리
   + 브라우저 닫기
   + 브라우저 강제 종료

* * *
![get_Ranking_by_Ticket_Category_guide](https://github.com/pnmGithub/get_Ranking_by_Ticket_Category.RPA-uipath/assets/149296871/78036414-caa6-4e44-9697-4029add33be7)
* * *

### REFrameWork Template ###
**Robotic Enterprise Framework**
