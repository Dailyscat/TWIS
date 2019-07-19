# Hedgehog란??

이름과 비밀번호를 사용하는 client-side Ethereum 지갑

## 특징

+ hedgehog는 작은 금전적인 수준에서 사용하도록 설계되었다.
+ 지갑의 복잡성을 숨기고 사용자가 거래를 확인하도록 강요하지 않음으로써 사용자 경험을 개선하였다.
+ dApp을 사용하기 위해서는 많은 기술 지식과 이해가 필요한데 이를 단순화 시켰다.

## Hedgehog는 모든 앱에 적합하지 않다.

유저 경험 개선에서 가장 눈에 띄는 부분은 거래에서 나온다. 이와 관련해서 hedgehog의 통용되는 규칙은 많은 돈과 연관된 app에서는 사용하지 않아야한다. 처음 wallet과 관련한 기술을 이용할 때 다리의 역할을 하는 도구로 사용하고 서비스를 키우기 위해서는 추후 안전하고 정교한 wallet으로 옮기는걸 추천한다.

## Hedgehog의 동작

hedgehog는 프론트엔드 어플리케이션에서 사용하는 패키지이다.
이 패키지는 유저의 엔트로피를 생성하고 관리하며 REST API 통해 서버와 db와 소통한다. 
hedgehog는 MyEtherWallet keystore file과 비슷한 일련의 집합을 만들어내고 개발자의 선택에 따라 db에 저장을 할 수 도 있다. 저장된 데이터는 사용자 이름, 암호 및 초기화 벡터에서 계산된 해시를 사용하여 검색이 가능하다.

### 지갑 생성

지갑은 BIP-39 규격에 따라 먼저 지갑 씨앗과 엔트로피를 생성함으로써 만들어진다. 그런 다음 엔트로피를 사용하여 BIP-32 사양에 명시된 경로에 따라 지갑 생성이 가능하다. 이 엔트로피는 외부 의존 없이 여러 세션에서 사용자 상태를 유지할 수 있도록 브라우저의 localStorage에 저장된다. 이 엔트로피를 사용하여 초기화할 때 이더리움 js-wallet의 지갑 객체가 생성되어 Hedgehog 클래스 내의 지갑 속성에 저장되어 상태 지속성이 활성화된다.

엔트로피 외에도, 헤지호그는 초기화 벡터(iv), 룩업키, cipherText를 생성한다. 이 세 가지 값은 데이터베이스에 안전하게 저장하고 서버에서 검색하여 사용자를 인증할 수 있다. iv는 각 사용자가 인증확보를 위해 생성한 랜덤 16진수 문자열이다. lookupKey는 미리 정의된 상시 초기화 벡터(데이터베이스에 저장되어 있는 iv와 동일하지 않음)와 결합된 사용자 이름 및 암호. 이 lookupKey는 cipherText 및 iv 값을 검색하는 데이터베이스에서 기본 키로 작용한다. certificateText는 iv와 함께 aes-256-cbc 암호와 scrypt를 이용한 iv의 조합에서 파생된 키를 사용하여 생성되며 엔트로피를 저장한다.