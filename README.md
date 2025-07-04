# profile_converter

한글 이력서 Word(.docx) 파일을 파싱하여 표준 JSON 포맷으로 변환하는 Python 기반 오픈소스 도구

---

## 📋 프로젝트 개요
- 다양한 한글 이력서 양식을 자동으로 분석하여, 인사담당자나 자동화 시스템이 활용할 수 있는 표준 JSON 데이터로 변환합니다.
- 정규표현식, 한글 자연어처리(NLP), 문서 인식 모델 등 최신 기술을 활용하여 다양한 레이아웃과 표현을 robust하게 지원합니다.

---

## 🚀 주요 기능
- **Word(.docx) 파일 파싱**: python-docx로 텍스트/표 추출
- **표준 JSON 변환**: 이름, 연락처, 학력, 경력, 기술 등 표준화된 JSON 스키마로 변환
- **한글 이력서 자동 인식 고도화**
  - 정규표현식/키워드 기반 섹션 구분
  - KoNLPy, KoSpaCy 등 한글 NLP로 엔티티 추출
  - Donut, LayoutLM 등 문서 인식 모델로 다양한 레이아웃 지원
- **CLI 제공**: 명령행에서 파일 입력/출력 지원
- **(선택) 웹 UI**: 간단한 웹 업로드/다운로드 기능(추후 지원)

---

## 🛠️ 기술 스택
- Python 3.x
- [python-docx](https://python-docx.readthedocs.io/)
- [KoNLPy](https://konlpy.org/), [KoSpaCy](https://github.com/entelecheia/kospacy)
- [Donut](https://huggingface.co/naver-clova-ix/donut-base-finetuned-cord-v2), [LayoutLM](https://huggingface.co/docs/transformers/model_doc/layoutlmv3)
- (선택) Flask 등 경량 웹 프레임워크

---

## ⚡ 설치 및 실행 방법
```bash
# 1. 저장소 클론
$ git clone https://github.com/younghokang/profile_converter.git
$ cd profile_converter

# 2. 가상환경 및 의존성 설치
$ python -m venv venv
$ source venv/bin/activate
$ pip install -r requirements.txt

# 3. CLI 실행 예시
$ python main.py --input sample_resume.docx --output result.json
```

---

## 📝 사용 예시
```python
from docx import Document
from konlpy.tag import Kkma
# ... (docs/korean_resume_parsing_guide.md 참고)
```
- 다양한 예시 및 상세 코드는 [docs/korean_resume_parsing_guide.md](docs/korean_resume_parsing_guide.md) 참고

---

## 📑 요구사항/설계 문서
- [docs/prd.txt](docs/prd.txt) : 전체 요구사항정의서(PRD)
- [docs/korean_resume_parsing_guide.md](docs/korean_resume_parsing_guide.md) : 한글 이력서 자동 인식 세부 가이드

---

## 🔗 참고 자료
- [python-docx 공식 문서](https://python-docx.readthedocs.io/)
- [KoNLPy 공식 문서](https://konlpy.org/)
- [KoSpaCy GitHub](https://github.com/entelecheia/kospacy)
- [Donut (HuggingFace)](https://huggingface.co/naver-clova-ix/donut-base-finetuned-cord-v2)
- [LayoutLM (HuggingFace)](https://huggingface.co/docs/transformers/model_doc/layoutlmv3)
- [Tesseract OCR](https://github.com/tesseract-ocr/tesseract)
- [EasyOCR](https://github.com/JaidedAI/EasyOCR)

---

## 🧑‍💻 기여 및 문의
- Pull Request, Issue 환영합니다!
- 문의: younghokang@gmail.com 