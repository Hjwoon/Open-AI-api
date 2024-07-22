## OpenAI API 사용하기
https://platform.openai.com/docs/guides/chat

- chatgpt api를 사용할 수 있는 가이드<hr>

### Open AI API 발급받기
https://platform.openai.com/api-keys
![img.png](img.png)

Create new secret key를 눌러 사용할 수 있는 API key 발급
![img_1.png](img_1.png)
- 발급받은 key를 통해 api와 통신을 하고 사용한 만큼 요금이 부과가 되는 방식

### OpenAI API를 사용할 수 있는 패키지 설치
```pip install openai```
- 해당 패키지로 api를 연결하여 ChatGPT를 이용

### API 연결하기
```python
import openai

# 발급받은 API 키 설정
OPENAI_API_KEY = "오픈AI에서 발급받은 인증키"

# openai API 키 인증
openai.api_key = OPENAI_API_KEY
```
- 발급받은 API 키 값을 ‘OPENAI_API_KEY’ 변수에 할당
- ‘openai.api_key’에 이 값 할당

### ChatGPT API 사용하기
```python
# 모델 - GPT 3.5 Turbo 선택
model = "gpt-3.5-turbo"

# 질문 작성하기
query = "텍스트를 이미지로 그려주는 모델에 대해 알려줘."

# 메시지 설정하기
messages = [{
    "role": "system",
    "content": "You are a helpful assistant."
}, {
    "role": "user",
    "content": query
}]
```

- model : ‘gpt-3.5-turbo’ 모델로 설정해준다. 이 모델은 ChatGPT 애플리케이션에서 사용되는 것과 같은 언어 모델이다.
- query : 질문을 입력한다.
- messages : 대화에 참여하는 여러 역할(‘system(시스템)’, ‘assistant(도우미)’, ‘user(사용자)’)과 메시지 내용을 설정할 수 있다. 일반적으로 대화는 시스템 메시지가 먼저 오고 그 다음에 사용자 및 어시스턴트 메시지가 번갈아 가며 오는 형식으로 구성된다.
  - 시스템 메시지 : ‘You are a helpful assistant.’와 같은 메시지로 챗봇에게 일종의 역할을 부여
  - 사용자 메시지 : 도우미에게 직접 전달하는 내용
  - 도우미 메시지 : 이전 응답을 저장하는 데 도움이 되며, 개발자가 원하는 동작의 예를 제공하기 위해 사용

```python
# ChatGPT API 호출하기
response = openai.ChatCompletion.create(model=model, messages=messages)
answer = response['choices'][0]['message']['content']
answer
```
openai.ChatCompletion.create()에 위에서 정의한 파라미터를 입력하고 ‘response’에 값을 할당

response[‘choices’][0][‘message’][‘content’]를 호출하면 응답 메시지를 확인할 수 있음