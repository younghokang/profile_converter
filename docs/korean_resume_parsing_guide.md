# 한글 이력서 자동 인식 세부 가이드

## 1. 세부 요구사항

### 1.1 섹션 구분 및 필드 추출
- 다양한 한글 이력서 양식에서 이름, 연락처, 학력, 경력, 기술 등 주요 섹션을 자동으로 구분해야 한다.
- 정규표현식, 키워드 기반 Rule을 설계하여 섹션 헤더(예: "학력", "경력", "기술")를 인식한다.
- 표, 리스트, 자유 양식 등 다양한 레이아웃을 지원한다.

### 1.2 한글 자연어처리(NLP) 활용
- KoNLPy, KoSpaCy 등 한글 NLP 라이브러리를 활용해 인명, 기관, 날짜, 직책 등 엔티티를 추출한다.
- 한글 형태소 분석, 명사/인명/기관명 추출, 날짜/기간 인식 기능을 포함한다.

### 1.3 문서 인식 모델 적용
- HuggingFace의 Donut, LayoutLM 등 문서 인식 모델을 활용해 표, 레이아웃, 이미지 기반 정보도 추출한다.
- OCR이 필요한 경우 Tesseract, EasyOCR 등과 연동 가능하다.

### 1.4 테스트 및 개선
- 다양한 한글 이력서 샘플을 수집하여 반복적으로 테스트하고, 규칙/모델을 개선한다.
- 예외 케이스(누락, 오타, 비표준 양식 등)도 robust하게 처리한다.

---

## 2. 예시 코드

### 2.1 python-docx로 텍스트/표 추출
```python
from docx import Document

doc = Document('sample_resume.docx')
# 본문 텍스트 추출
for para in doc.paragraphs:
    print(para.text)
# 표 데이터 추출
for table in doc.tables:
    for row in table.rows:
        print([cell.text for cell in row.cells])
```

### 2.2 한글 섹션 구분 Rule 예시
```python
import re
sections = {}
section_names = ["이름", "연락처", "학력", "경력", "기술"]
current = None
for line in lines:  # lines: 이력서 각 줄 리스트
    for name in section_names:
        if re.match(f"^{name}", line):
            current = name
            sections[current] = []
            break
    if current:
        sections[current].append(line)
```

### 2.3 KoNLPy로 한글 엔티티 추출
```python
from konlpy.tag import Kkma
kkma = Kkma()
text = "홍길동은 2020년 3월부터 삼성전자에서 근무했다."
print(kkma.nouns(text))  # 명사 추출
print(kkma.pos(text))    # 품사 태깅
```

### 2.4 KoSpaCy로 한글 NER
```python
import kospacy
nlp = kospacy.load()
doc = nlp("홍길동은 2020년 3월부터 삼성전자에서 근무했다.")
for ent in doc.ents:
    print(ent.text, ent.label_)
```

### 2.5 Donut 문서 인식 모델(HuggingFace)
```python
from transformers import DonutProcessor, VisionEncoderDecoderModel
from PIL import Image
import torch

processor = DonutProcessor.from_pretrained('naver-clova-ix/donut-base-finetuned-cord-v2')
model = VisionEncoderDecoderModel.from_pretrained('naver-clova-ix/donut-base-finetuned-cord-v2')
image = Image.open('sample_resume.png').convert('RGB')
task_prompt = '<s_cord-v2>'
inputs = processor(image, task_prompt, return_tensors='pt')
outputs = model.generate(**inputs)
parsed = processor.batch_decode(outputs, skip_special_tokens=True)[0]
print(parsed)
```

---

## 3. 참고 기술 자료/링크
- [python-docx 공식 문서](https://python-docx.readthedocs.io/)
- [KoNLPy 공식 문서](https://konlpy.org/)
- [KoSpaCy GitHub](https://github.com/entelecheia/kospacy)
- [Donut (HuggingFace)](https://huggingface.co/naver-clova-ix/donut-base-finetuned-cord-v2)
- [LayoutLM (HuggingFace)](https://huggingface.co/docs/transformers/model_doc/layoutlmv3)
- [Tesseract OCR](https://github.com/tesseract-ocr/tesseract)
- [EasyOCR](https://github.com/JaidedAI/EasyOCR) 