# PROJECT : myhoneytrip🐝

### • 프로젝트 소개
>**마이리얼트립을 모티브로 한 항공권 예매사이트**  
마이허니트립은 마이리얼트립을 모티브로 허니문 항공권 예약과 관련된 사용자의 소셜 로그인(kakao)과 검색결과 기반 상세페이지, 항공권 예약, 마이페이지까지 이어지는 커머스 사이트의 기본적인 Flow 기능을 구현한 사이트입니다.

### • 작업기간 : 8월 1일 ~ 8월 12일 (12일)

### • 팀원 소개
> wecode 35기 2차 프로젝트 마이허니트립 TEAM

  **FE** | 구단희, 김익현, 신수정, 이강철(PM)  
  **BE** | 황유정, 음정민, 안상현
  
### • 사용기술 및 협업 도구  
> FE: Javascript, React, React-Router, styled-components  
>     library : slick slide, react date picker, mui modal, mul table  
> BE: Phython, Django, MySQL, MINICONDA3, POSTMAN  
> Common : Git&Github, AWS 
> Comunication : Notion, Slack, Trello, Github 

### • 구현기능 및 사용 기술 소개 

```
구단희 : 로그인 페이지, 메인 배너 Carousel, Nav, Footer
김익현 : 로딩 페이지, 항공편 예약 및 마이페이지(예약 확인 및 취소)
신수정 : 검색 상품 리스트 페이지, 검색 결과 filtering
이강철 : 메인 검색창 (캘린더 라이브러리), 최근 검색 항공편 배너 노출
```

### • Site DEMO

https://user-images.githubusercontent.com/99232122/184281715-92bcc9a4-fe11-4405-9c61-a79ed58b75f0.mov  

<http://2nd-myhoneytrip.s3-website.ap-northeast-2.amazonaws.com/>

#### 1. 카카오 로그인 API  
- useEffect를 활용한 인가코드 발급을 서버로 전달 (FE 구단희)  

#### 2. 메인 페이지. 
- Slick library를 활용한 Carousel (FE 구단희). 
- Search Bar 클릭 시 모달 및 달력 모달창 library 구현 (FE 이강철)  
- 최근 검색 결과 상위 배너로 노출 (FE 이강철)  
- mock data로 호출한 추천상품 노출 (FE 이강철)  

####  3. 검색 상품 리스트 페이지
- 데이터를 불러오기 전 로딩페이지 구현 (FE 김익현) 
<img width="1354" alt="스크린샷 2022-08-14 오후 7 33 51" src="https://user-images.githubusercontent.com/101634412/184532977-602171c4-ec29-4fbe-847c-69f9da4b0a22.png">
  메인페이지로 부터 navigate('',state: {출발,도착데이터})로 넘겨받은 데이터를 location.state를 활용해 로딩될 동안 지루 할 수 있는 페이지를 보다 활동적으로 구현
  
  ```jsx
   const isData = Object.keys(flightData).length !== 0;
  if (!isData) return <Loading loadingData={loadingData} />;
  ```
  
- useLocation을 활용해 querystring을 받아와 서버 데이터 요청 (FE 신수정)  
- selectbox를 통한 filter 기능 구현 (feat. searchParams) (FE 신수정)  
- useNavtigate안에 데이터를 담아 다음 페이지로 전달 (FE 신수정)  

#### 4. 구매 상품 확인 페이지  
<img width="1158" alt="스크린샷 2022-08-14 오후 7 40 22" src="https://user-images.githubusercontent.com/101634412/184533143-7d2a3077-9d52-4947-a486-e4fd257a5979.png">
- useLocation을 활용해 이전 페이지에서 보내준 데이터 시각화 (FE 김익현)  

```jsx
useEffect(() => {
    setTicketData(location.state);
  }, [location.state]);
```

- useNavtigate안에 데이터를 담아 다음 페이지로 전달 (FE 김익현) 

```jsx
onClick={() =>
            localStorage.getItem('token') === null
              ? (alert('로그인이 필요합니다.'), navigate('/login'))
              : navigate('/passengerdata', {
                  state: {
                    Departure_Data: Departure_Data,
                    Arrive_Data: Arrive_Data,
                  },
                })
```

#### 5. 탑승객 및 예약자 정보 입력 페이지
![2nd-myhoneytrip s3-website ap-northeast-2 amazonaws com_passengerdata](https://user-images.githubusercontent.com/101634412/184533211-7c0f898d-73bc-40a8-a400-f63d31ddaa53.png)

- 가상의 배열을 만들어 탑승객 수만큼 입력 페이지 생성 (FE 김익현)  

```jsx
[...Array(Number(Departure_Data.passengers))].map((n, idx) => {
            const handlePassengerData = e => {
              const { name, value } = e.target;
              setPassengerData({
                ...passengerData,
                [idx]: { ...passengerData[idx], [name]: value },
              });
            };
```
- 서버로 전달하기 위한 데이터 가공 (FE 김익현)  

```jsx
Object.values(passengerData).map(object =>
    PassengerDataArr.push({
      first_name: object.first_name,
      last_name: object.last_name,
      gender: object.gender,
      birthday:
        object.year + '-' + overTen(object.month) + '-' + overTen(object.day),
    })
  );
```


#### 6. 마이 페이지 - 예약확인 및 예약취소
<img width="1066" alt="스크린샷 2022-08-14 오후 7 45 59" src="https://user-images.githubusercontent.com/101634412/184533286-9b24d40c-80b9-49f7-9ce7-613bde13e0d6.png">

- 현재 탭에 따른 데이터 요청 예약취소 시 patch를 사용해 데이터 상태 변경 (FE 김익현)  

```jsx
const cancelTravel = () => {
    fetch('http://54.180.2.226:8001/bookings/mytrip', {
      method: 'PATCH',
      headers: { Authorization: localStorage.getItem('token') },
      body: JSON.stringify({ booking_id: booking_id }),
    })
      .then(res => res.json())
      .then(
        data =>
          data.message === 'CANCEL SUCCESS' &&
          (alert('취소가 완료되었습니다.'), navigate('/checkreservation'))
      );
  };
```

#### 7. NAV 
- 로그인/로그아웃 상태변경 및 마이페이지 이동 구현 (FE 구단희)  
 
#### 8. Footer
- 상수데이터 활용하여 구성 (FE 구단희)  

### • 참고
#####
이 프로젝트는 마이리얼트립 사이트를 참조하여 학습 목적으로 만들었습니다.  
실무수준의 프로젝트이지만 학습용으로 만들었기 때문에 이 코드를 활용하여 이득을 취하거나 무단 배포할 경우 법적으로 문제될 수 있습니다.  
이 프로젝트에 사용된 모든 영상 및 이미지, 폰트 정보는 저작자 표기가 필요하지 않는 Royalty-free를 사용했습니다.  
