# 재밌게도 뭘 할지 적는 게 아니라 뭘 했는지 적는 
## 결과
### 기본 모양과 반응형
![image](https://user-images.githubusercontent.com/100043324/214741022-58794706-c257-405d-9eff-fb6f6e0f8ab3.png)

### 모바일
![image](https://user-images.githubusercontent.com/100043324/214741165-b7da51e6-e097-4ae7-aef1-780ddec5b79d.png)
![기본과반응형](https://user-images.githubusercontent.com/100043324/214219226-65d9a913-ceaa-47a3-9763-6104a02d4789.gif)

### 스크롤
![스크롤)](https://user-images.githubusercontent.com/100043324/214219228-56d430fa-b398-423d-9790-d001c683eac4.gif)

## 디자인
FIGMA로 간단하게 디자인 했습니다.

https://www.figma.com/file/AQ9NFkDQ1xlwc2gjGiK1e8/didilist?node-id=0%3A1&t=O63pEtTKLyNaaV1H-1

오래된 미국 애니메이션의 신나지만 정신없는 디자인을 바탕으로 만들었습니다.

심슨가족에 나오는 이치와 스크래치 같은 느낌이지만 조금은 다른 색을 원했습니다.

원하는 폰트는 금방 찾을 수 있었지만 상상으로만 하던 색감을 찾는데 시간이 꽤나 오래 걸렸지만 결과적으로 만족스럽습니다.

그림자 효과 특히 제목에 두번 들어간 그림자는 촌스러운 느낌을 줘서 좋습니다.

![image](https://user-images.githubusercontent.com/100043324/214220189-537466ee-1045-4184-8c86-7e3c109ce695.png)

* 추가사항 
큰 박스를 두지 않고 가운데 딱 맞는 박스로 되도록 디자인을 변경했습니다.

사실 실수로 이렇게 됐었는데 더 간결해 보이고 풍선껌 같은 느낌이 잘 어울려서 적용했습니다.
![image](https://user-images.githubusercontent.com/100043324/214741507-2bd64ed3-7986-4a22-9cee-f205196d92df.png)


## 기능
댓글다는 기능처럼 새로운 박스를 만들어 위에서부터 쌓이도록 했다.

List.js
```js
import React, { useState } from "react";
import ItemList from "./ItemList";

const List = (props) => {
  let [item, setItem] = useState(''); //입력중인 글 input태그에서 onChange 사용해 담기
  let [feedItems, setFeedItems] = useState([]); //글 리스트를 담으려고 만듬
  let [isValid, setIsValid] = useState(false); // 게시 가능여부(유효성 검사)
  let post = (e) => { // 유효성 검사 통화하고 실행시 발생하는 함수
    const copyFeedItems = [...feedItems]; //전개 연산자를 통해 feedItems에 담겨 있는 댓글 받아오기
    copyFeedItems.push(item); //기존 댓글 배열이 담겨있는 copyFeedItems에 입력한 item 값을 push
    setFeedItems(copyFeedItems); // 입력한 item이 담겨있을 feedItem 값을 setFeedItem로 변경
    setItem(''); //글창을 빈문자열로 초기화
  };
  const onEnter=(e)=>{
    if(e.key == 'Enter'){ //Enter 누르면 post실행하기
      post();
    }
  }
  return (
    <>
      <div className="list_container" >
        <div className="item_list">
          <input
            type="text"
            className="item_input"
            placeholder="오늘 무엇을 했는지 알려주세요!"
            onChange={(e) => {
              setItem(e.target.value); //입력창 상태가 변할 때 마다 setItem통해 item 값 바꾸기
            }}
            onKeyUp={(e)=>{ // 키를 눌렀다가 떼었을 때 길이가 0을 넘는지 유효성 검사 결과 값 담기
              e.target.value.length>0 ? setIsValid(true):setIsValid(false);
            }}
            onKeyDown={onEnter}
            value={item}
          />
          <button
            className="button_"
            onClick={()=>{post()}} // 클릭하면 위에 선언한 post 함수를 실행해서 feedItems에 담겨 다시 렌더된다.
            disabled={isValid?false:true} // 빈칸이면 게시 불가
          >+</button>
        </div>
        {feedItems.map((itemArr, i) => { // 반복적으로 추가되는걸 하나하나 담을 예정
          return (
              <ItemList //댓글을 담을 컴포넌트
                  userItem={itemArr} //feedItems에 담겨있는 댓글 배열을 담는다.
                  key={i} //순서
              />
          );
        })}

      </div>
    </>
  );
};

export default List;

```

ItemList.js
```js
import React from "react";

const ItemList = (props) => {

  return (
      <div className="item_list">
          < div className="items_">{props.userItem}</div>
      </div>
  );
};

export default ItemList;
```

항상 느끼는거지만 이리저리 가지고 노는게 아주 재밌습니다.


## 나의 하루 시나리오 게임

### 결과
![시나리오](https://user-images.githubusercontent.com/100043324/214742726-b4c0f568-967a-4e91-9f5e-8a3d69a568f2.gif)

### 시나리오
기상부터 잠들기 전까지의 일과를 선택 시나리오 게임으로 만들어 보았다.

- 처음으로는 기상 시나리오가 주어지고 두개의 선택지가 주어진다.
- 선택지를 누르고 확인을 하면 해당되는결과가 주어지고 시나리오 14까지 도달하면 성공
    - if문을 사용해 어떤 것이 선택 되었는지 확인한다.
    - 한 시나리오가 끝날때 마다 bad 수치를 체크해 시나리오 실패여부를 체크한다.
- 실패
    - bad가 5 이상이면 실패
    - bad가 -4 이하이면 기상 으로 돌아와 다시 플레이 하게 된다.

## 1. 기상

선택지

- 7:30에 울리는 알람을 듣고 일어난다.
    - 2.로 이동
- 7:30에 울리는 알람을 끄고 다시 잔다.
    - 개들이 오줌마렵다고 낑낑대서 얼마 안가 깼다. 개들을 뒷마당으로 보내고 커피를 내린다.
    3. 으로 이동

## 2.  주방

- 개들 뒷마당으로 보내고 커피내리기
    - 3.으로 이동
- 커피내리기
    - 개들이 소파옆에 오줌을 쌌다.
    - bad+1
    - 3.으로 이동

## 3. 거실

- 저녁에 잊은 커밋을 한다.
    - 커밋을 해서 그런지 자신있는 수정이 가능했다. 4로 간다.
- 커밋없이 그냥 작업한다.
    - 외계인의 습격으로 컴퓨터가 오류가 나서 다시 시작해야 했다
    - bad+2

## 4. 동물병원

- 두부 백신과 건강검진을 위해 동물병원에 간다.
    - 두부는 이 동물병원에서 기존 진료기록이 없어서 오늘은 백신을 맞을 수 없다고 한다.
    - bad+1
    - 5.로 이동
- 추워서 귀찮은데 가지 않는다.
    - 체크를 언젠가는 해야하긴 한다는 생각이 가시질 않아서 신경쓰인다.
    - bad+1
    - 5.로 이동

## 5. 공부방

와이프가 거실에서 회의중이라 공부방으로 왔다. 

- 집중해서 프로젝트를 한다.
    - bad -1
    - 6으로 이동
- 혼자있으니까 눈치보지 않고 핸드폰 게임한다.
    - bad+1
    - 6으로 이동

## 6.점심시간

- 요리는 즐겁다 맛있는 점심을 먹고 휴식을 취한다.
    - 7로 이동

## 7. 공부방

- 집중해서 프로젝트를 한다.
    - bad-1
    - 8로 이동
- 배부르고 혼자 있으니까 낮잠을 잔다.
    - 아뿔사 밤이 되어 버렸다.
    - bad +10

## 8.기능 완성

기능 구현에 성공했다. 

- 와이프에게 자랑한다.
    - 구현중에 에러를 발견해 수정할 수 있었다.
    - bad -1
    - 9로 이동
- 와이프에게 자랑하지 않는다.
    - 9이동

## 9.저녁식사 선택

- 파파이스를 선택한다.
    - 10으로 이동
- 키키리키(페루비안 치킨)을 선택한다.
    - 와이프가 맥주 마시고 싶다면서 파파이스를 먹자고 한다.
    - 10으로 이동

## 10.마트

- 라거를 고른다.
    - 카스를 찾았다 꿈만같다!!
    - bad-10
- 에일을 고른다.
    - 오래간만에 호가든을 마시기로 했다. 미국 맥주인 블루문과 비슷하다고 와이프에게 TMI를 한다.
    - 11로 이동

## 11. 저녁식사

거실에서 영화를 보며 저녁식사를 한다. 호가든 작은병 한병은 조금 모자라는 느낌이 든다.

- 한 병 더 딴다.
    - bad+2
    - 12로 이동
- 한 병 더 따지 않는다.
    - 12로 이동

## 12. 영화

개들과 소파에 누워서 영화를 보고 있다.

- 이럴때가 아닌데!! 프로젝트를 마무리 하러 간다.
    - 술기운에 집중이 되지 않았다. 시간낭비를 한 것 같다.
    - bad+1
    - 13으로 간다.
- 편하게 영화를 끝까지 본다.
    - 좋은 휴식을 취한 것 같다.
    - bad-1
    - 13으로 간다.

## 13. 실행결과 녹화

- 여러번 실행해 보았는데 에러가 하나도 발견되지 않았다 꿈만 같다!
    - bad - 10
- 몇개의 에러가 발견되었고 수정했다.
    - 14로 간다.

## 14. 성공엔딩

- 축하합니다 오늘 하루를 기똥차게 보냈습니다.
    - 다시하기

## 20.실패엔딩1

- 실패 제 시간안에 끝내지 못했다
    - 다시하기

## 21.실패엔딩2

- 실패 꿈만 같은 하루였군요. 꿈에서 깹니다.
1로 돌아갑니다.


## 주요 기능

```js
import React, { useState } from "react";

const SelectDay = () => {
const [scenario,setScenario] =useState('start'); //시나리오 이동을 위한 state
const changeScenario=(props)=>{ // props 로 받아와서 바뀐다.
  setScenario(props)
};
const [bad,setBad]=useState(0); //게임 오버를 만들기 위한 bad 수치 0부터 시작
const addBad=(e)=>{ //e로 숫자를 받아와서 뿌려준다
  setBad(bad+e)
};
const minusBad=(e)=>{
setBad(bad-e)
};
const resetBad=()=>{ //게임이 끝나고 리셋
  setBad(bad===0)
}
const checkBad=()=>{ // 만약 bad가 3이상이면 배드엔딩1 -2이하이면 배드엔딩2로 간다.
  if(bad>=3){
    changeScenario("badEnding01")
  }else if(bad<=-2){
    changeScenario("badEnding02")
  }
};
         
  return (

        <div className="game_item_list">
          {
            {
              start:
                <div className="items_"><div className="game_title">침실</div>
                  <div className="intro_">오전 7:30, 손목시계에서 알람이 울린다.</div>
                  <button className="game_item" onClick={()=>{changeScenario("s2");}}>알람을 듣고 일어난다.</button> <br />
                  <button className="game_item" onClick={()=>{changeScenario("s1_1");}}>알람을 끄고 다시 잔다.</button>
                </div>,
              s1_1:
                <div className="items_"><div className="game_title">침실</div>
                <div className="intro_">개들이 오줌마렵다고 낑낑대서 얼마 안가 잠에서 깼다 </div>
                  <button className="game_item" onClick={()=>{changeScenario("s2"); checkBad();}}>화장실을 사용하고 주방으로 향한다.</button> <br />
                  <button className="game_item" onClick={()=>{changeScenario("s2"); checkBad();}}>졸린 눈을 비비며 주방으로 향한다.</button>
                </div>,
              s2:
                <div className="items_"><div className="game_title">주방</div>
                <div className="intro_">아침에 일어나면 커피를 마셔야 한다.</div>
                  <button className="game_item" onClick={()=>{changeScenario("s3"); checkBad(); }}>개들을 뒷마당으로 보내고 커피를 내린다.</button> <br />
                  <button className="game_item" onClick={()=>{changeScenario("s2_1"); addBad(1);}}>커피를 내린다.</button>
                </div>,
              s2_1:
                <div className="items_"><div className="game_title">주방</div>
                <div className="intro_">개들이 소파옆에 오줌을 쌌다.</div>
                  <button className="game_item" onClick={()=>{changeScenario("s3"); addBad(1); checkBad();}}>오줌을 대충 닦고 거실로 향한다.</button> <br />
                  <button className="game_item" onClick={()=>{changeScenario("s3"); minusBad(1); checkBad();}}>오줌을 깨끗하게 닦고 거실로 향한다.</button>
                </div>,
              .
              .
              .
              .
            }[scenario]//scenario 받아주기
          }
        </div>

  );
};

export default SelectDay;
```


상상하고 구현하는데 재밌는 시간을 보냈다.
State를 더 잘 사용하게 되었습니다.

props를 연결 연결해서 자식 부모 주고 받고 하는 연습을 더 해봐야 할 것 같습니다.
