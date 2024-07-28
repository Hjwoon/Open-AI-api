## AI 맞춤 회고 분석 시스템

### 1. 자동 요약 및 인사이트 제공
- 회고 데이터를 분석하여 **커밋 분석**, **코드 분석**, 이에 따른 **문제점**, **개선 사항**을 자동 요약

### 2. 트렌드 분석
- 이전 프로젝트들과 비교하여 반복되는 문제나 성과를 분석하고 트렌드 제시.

### 3. 개인화된 학습 추천
- 개인의 학습 스타일과 프로젝트에서 도출된 개선 사항을 바탕으로 학습 자료나 강좌 추천. (자바 코드 사용 프로젝트 -> 파이썬 라이브러리로 대체, 파이썬 공부 권유)

## 인터랙티브 회고 챗봇

### 1. 자동 회고 세션 진행
- 정해진 주기마다 회고 세션을 자동으로 주최하고, 팀원들의 응답을 바탕으로 회고 내용 정리. (추후에 커뮤니티를 팀 단위로 생성하면 어떨까 함)

### 감정 분석 및 팀 건강 체크

### 1. 감정 분석
- 회고 내용에서 팀원들의 감정을 분석하여 팀의 사기를 평가하고, 필요시 조치 제안.

### 2. 팀 건강 체크리스트
- 프로젝트 진행 중 팀의 건강 상태를 체크할 수 있는 설문을 자동으로 생성하고 분석.

## 프로젝트 성과 예측 및 경고 시스템

### 1. 성과 예측
- 과거 데이터와 현재 진행 상황을 바탕으로 프로젝트의 성과 예측.

<hr>

### 구현 예시
#### 1. 지능형 회고 분석 시스템
```python
import openai

# ChatGPT API를 통해 회고 데이터 요약 및 인사이트 제공
def generate_summary_and_insights(retrospective_data):
    prompt = f"다음은 프로젝트 회고 내용입니다:\n\n{retrospective_data}\n\n주요 성과, 문제점, 개선 사항을 요약하고 인사이트를 제공해 주세요."
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[
            {"role": "system", "content": "You are an expert in project management and retrospectives."},
            {"role": "user", "content": prompt},
        ],
        temperature=0.7,
    )
    return response.choices[0].message['content']

# 예시 회고 데이터
retrospective_data = """
프로젝트 목표는 달성했으나, 일정이 지연되었습니다. 커뮤니케이션 문제로 인해 몇 가지 기능이 누락되었고, 테스트 커버리지가 부족했습니다.
잘된 점은 팀원들이 자발적으로 문제를 해결하려는 노력이 있었다는 점입니다. 향후에는 커뮤니케이션 도구를 개선하고, 테스트 자동화를 강화할 계획입니다.
"""

summary_and_insights = generate_summary_and_insights(retrospective_data)
print(summary_and_insights)
```

#### 2. 인터랙티브 회고 챗봇
```python
from flask import Flask, request, jsonify
import openai

app = Flask(__name__)

@app.route('/retrospective', methods=['POST'])
def retrospective():
    data = request.json
    retrospective_data = data.get('retrospective_data')

    prompt = f"다음은 프로젝트 회고 내용입니다:\n\n{retrospective_data}\n\n주요 성과, 문제점, 개선 사항을 요약하고 피드백을 제공해 주세요."
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[
            {"role": "system", "content": "You are an expert in project management and retrospectives."},
            {"role": "user", "content": prompt},
        ],
        temperature=0.7,
    )
    summary_and_feedback = response.choices[0].message['content']
    return jsonify({"summary_and_feedback": summary_and_feedback})

if __name__ == '__main__':
    app.run(debug=True)
```

<hr>

#### 역할 부여 예시

```markdown
# 시스템 메시지
"You are an expert in project management and retrospectives. Your role is to assist in generating comprehensive retrospective reports by analyzing project data from GitHub, identifying key insights, and providing actionable feedback."

# 사용자 메시지
"Please provide your GitHub username and the repository name for the project you want to analyze."

# 도우미 메시지
"Based on the provided GitHub repository data, you should retrieve and analyze the project information, summarize the key points, identify recurring trends, and offer tailored feedback for improvement. For example:

1. **프로젝트 기본 정보**
   - 프로젝트 이름: 자동으로 GitHub에서 가져옵니다.
   - 프로젝트 기간: 레포지토리의 첫 커밋 날짜와 마지막 커밋 날짜를 기준으로 합니다.
   - 참여 인원 및 역할: 레포지토리의 기여자 목록을 가져옵니다.

2. **목표 및 성과**
   - 목표: 프로젝트의 README 파일에서 목표나 설명을 추출합니다.
   - 성과: GitHub Issues와 Pull Requests를 분석하여 주요 성과를 파악합니다.

3. **진행 과정 리뷰**
   - 마일스톤: GitHub 마일스톤 기능을 활용하여 진행 상황을 평가합니다.
   - 이슈: GitHub Issues를 분석하여 주요 이슈와 해결 여부를 파악합니다.

4. **성과 및 실패 분석**
   - 잘된 점: 기여자들의 커밋 메시지와 피드백을 분석합니다.
   - 실패: 미해결 이슈와 PR을 분석합니다.

5. **학습 및 기술적 검토**
   - 학습 내용: 프로젝트 중 도입된 새로운 기술이나 도구를 README나 커밋 메시지에서 추출합니다.
   - 기술적 도전: 주된 기술적 문제와 그 해결 방법을 분석합니다.

6. **개인 성장 및 학습**
   - 개인적 성장: 기여자별 커밋과 코드 리뷰 활동을 분석하여 개인의 성장과 기여도를 평가합니다.
   - 향후 개선: 프로젝트 리뷰를 바탕으로 개선 사항을 도출합니다.

이와 같이 GitHub 데이터를 구조화하여 분석하고, 주요 성과와 개선 사항을 요약하여 사용자에게 제공하십시오."
```

<hr>

## 회고 리포트 기능 정리

### 1. 사용자 분석 문장 생성 

### 2. 사용자에게 최적화된 리포트 항목 타이틀 생성 후 답변 요청

### 3. 자동 요약 및 인사이트 제공
- 회고 데이터를 분석하여 **커밋 분석**, **코드 분석**, 이에 따른 **문제점**, **개선 사항**을 자동 요약

### 4. 트렌드 분석
- 이전 프로젝트들과 비교하여 반복되는 문제나 주제, 성과를 분석하고 트렌드 제시.

### 5. 학습 추천
- 개인의 학습 스타일과 프로젝트에서 도출된 개선 사항을 바탕으로 학습 자료나 강좌 추천. 
- 자바 코드 사용 프로젝트 -> 파이썬 라이브러리로 대체, 파이썬 공부 권유

### 6. 회고 리포트 커뮤니티
- 정해진 주기마다 회고 세션을 자동으로 주최하고, 팀원들의 응답을 바탕으로 회고 내용 정리. 
- 추후에 커뮤니티를 팀 단위로 생성하면 어떨까 함