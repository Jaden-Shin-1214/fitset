# Fitset

레이어드 가상 피팅을 위한 인터랙티브 코디 보드 (GenAI × HCI 텀프로젝트 프로토타입).

## 흐름
1. **세트 구성**: 옷장에서 옷을 누르거나 보드로 끌어오면 카테고리 자리로 날아가 놓입니다. 보드 위 옷에 커서를 올리면 실루엣 오른쪽 위에 × 버튼이 나타나고, 누르면 뺄 수 있습니다. 샘플 코디 보드를 넣으면 6벌이 원래 위치에서 한 벌씩 날아옵니다. 착샷을 올리면 Gemini가 옷을 골라 펼쳐 놓습니다 (API 키 필요).
2. **조정**: 옷마다 둥근 사각 카드가 생깁니다. 카드를 누르면 색 · 사이즈 · 핏 · 레이어 · 착용 방식 시트가 아래로 열립니다. 카드 안의 색 칸(기본값은 옷 색)과 사이즈 칸(기본값은 빈칸)에 바뀐 내용이 바로 표시됩니다. 색은 원본 명암을 유지한 채 다시 칠합니다.
3. **입혀보기**: 사람 사진을 넣으면 배경을 지우고 RGBA / 알파로 보여줍니다. 단색 배경은 브라우저 안에서 바로 처리하고, 복잡한 배경은 Gemini로 처리합니다. 조정한 세트로 Gemini 입혀보기 요청을 보낼 수 있습니다.

## 실행
정적 파일만 사용합니다. 저장소 루트에 그대로 올리고 GitHub Pages(Settings → Pages → Deploy from branch)를 켜면 됩니다. 로컬 확인: `python3 -m http.server` 실행 후 `http://localhost:8000`

## AI 연결 (Gemini 또는 OpenAI)
오른쪽 위 "AI 연결"에서 서비스를 고르고 API 키를 넣으면 브라우저에서 해당 API로 직접 요청합니다. 키는 그 브라우저의 localStorage에만 저장됩니다. 공개 저장소에 키를 커밋하지 마세요.
- **Gemini**: 이미지 `gemini-2.5-flash-image`, 분석 `gemini-2.5-flash` (`generativelanguage.googleapis.com`). 이미지 생성 모델은 무료 등급 한도가 0일 수 있어 결제 연결이 필요합니다.
- **OpenAI**: 이미지 `gpt-image-1` (`/v1/images/edits`, 여러 참조 이미지 + 투명 배경), 분석 `gpt-4.1-mini` (`/v1/chat/completions`).
- 모델 이름은 같은 창에서 바꿀 수 있습니다.

## 파일
- `index.html`: 앱 전체 (단일 파일)
- `assets/garments/*.png`: 배경을 지운 옷 이미지 6장 (RGBA)
- `assets/sample-board.jpg`: 원본 코디 보드
- `assets/sample-outfit.jpg`: 샘플 착장 사진 (데모에서 옷이 이 사진 속 위치에서 빠져나옴)
- `data/garments.json`: 옷 메타데이터 (`id, category, name, en, src, w, h, color, origin.box`)

## 알려진 한계
- 착장 분리, 누끼, 입혀보기의 Gemini / OpenAI 연동 코드는 실제 키로 아직 끝까지 검증하지 않았습니다.
- 단색 배경 지우기는 테두리 색 기반 근사라서 배경이 복잡하면 깨집니다.
- 다시 칠하기는 옷 전체에 적용되므로 벨트 버클 같은 부속도 함께 색이 바뀝니다.
