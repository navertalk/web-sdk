# 네이버톡톡 Web SDK 

네이버톡톡 Web SDK는 Javascript 제공되며, 운영하는 사이트에 삽입하여 네이버 톡톡 연동기능을 구현할 수 있습니다.
현재 구현할 수 있는 기능은 다음과 같으며, 앞으로 계속 추가될 예정입니다 😃 

- 알림받기 버튼 표시
- 알림받기 동작 구현
- 알림받기 혜택 표시 동작 구현

## 시작하기

네이버톡톡 기능을 구현하려는 페이지에 sdk 스크립트를 추가해주세요.
**[톡톡파트너센터](https://partner.talk.naver.com/) > 설정 > 알림받기 버튼만들기 > 코드 만들기** 에서 생성한 코드를 삽입합니다.

```html
<script type="text/javascript" src="https://ua.talk.naver.com/dist/sdk.js"></script>
<script>
NaverTalkSDK.init({
    talkId: '${발급된 톡톡아이디로 대체}',
    channelNo: '${발급된 채널번호로 대체}'
})
</script>
```

## 개발하기

### 커스텀 알림받기 동작 구현

`NaverTalkSDK.Friend.addFriend` 함수를 호출하면 톡톡 알림받기 페이지로 이동합니다.

```html
<button onclick="onClickFriend()">톡톡 알림받기</button>
<script>
    // 네이버톡톡 알림받기 페이지로 이동합니다.
    function onClickFriend() {
        NaverTalkSDK.Friend.addFriend()
    }
</script>
```

### 커스텀 알림받기 혜택 동작 구현

 `NaverTalkSDK.Friend.getBenefit` 호출하여 현재 설정한 혜택정보를 로드할 수 있습니다. 
 이를 활용하여 알림받기 혜택 팝업을 직접 구현할 수 있습니다. 

```html
<button onclick="onShowBenefit()">톡톡 알림받기 혜택 표시</button>
<div id="benefit-content">혜택 조회 내용으로 대체됩니다.</div>
<script>
    // 혜택정보를 표시합니다.
    function onShowBenefit() {
        NaverTalkSDK.Friend.getBenefit().then(function(benefit) {
            var el = document.getElementById('benefit-content')

            if (benefit) {
                el.innerHTML = benefit.benefitContent
            } else {
                el.innerHTML = '혜택이 존재하지 않습니다.'
            }
        })
        // Promise 객체를 응답합니다. 따라서 catch, finally 핸들러를 추가할 수 있습니다.
    }
</script>
```

### 기본 알림받기 버튼 렌더링

SDK 스크립트 설치 후 알림받기 마크업을 페이지에 붙이면, 별도의 코드 없이 알림받기 버튼이 렌더링됩니다.
단, 마크업이 React 혹은 Vue 등을 활용하여 브라우저에 동적으로 마운트되는 경우 동작하지 않습니다. 
동적으로 마운드 되는 경우는 아래 **동적 알림받기 버튼 렌더링** 항목을 참고해주세요.

```html
<!-- "톡톡파트너센터 > 설정 > 알림받기 버튼만들기" 에서 생성한 버튼 마크업을 삽입합니다. -->
<div class="navertalk-friend-button" data-talk-id="..." data-channel-no="..."><div>
```

### 동적 알림받기 버튼 렌더링

React나 Vue를 통해 브라우저에서 동적으로 마운트되는 알림받기 버튼의 경우, 자동으로 렌더링이 되지 않습니다.
다음과 같이 DOM 마운트 후 직접 `NaverTalkSDK.Friend.renderButton` 함수를 호출해야합니다.

#### React 예제

```jsx
const MyComponent = () => {
    useEffect(() => {
        // DOM 마운트 후 알림받기 버튼을 랜더링ㅇ합니다.
        NaverTalkSDK.Friend.renderButton()
    }, [])

    return (
        <div>
            {/* "톡톡파트너센터 > 설정 > 알림받기 버튼만들기" 에서 생성한 버튼 마크업을 삽입합니다. */}
            <div className="navertalk-friend-button" data-talk-id="..." data-channel-no="..."><div>
        </div>
    )
}
```

#### Vue 예제

```vue
<script>
export default {
  mounted() {
    // DOM 마운트 후 알림받기 버튼을 랜더링ㅇ합니다.
    NaverTalkSDK.Friend.renderButton()
  }
}
</script>

<template>
  <!-- "톡톡파트너센터 > 설정 > 알림받기 버튼만들기" 에서 생성한 버튼 마크업을 삽입합니다.  -->
  <div className="navertalk-friend-button" data-talk-id="..." data-channel-no="..."><div>
</template>
```

## 버그 및 개선사항 제보

버그 및 개선사항은 이슈로 등록해주시면 감사하겠습니다. 🙏 

