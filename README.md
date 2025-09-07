# 🎮 테트리스 게임 (Tetris Game)

HTML5, CSS3, JavaScript로 구현된 웹 기반 테트리스 게임입니다.

![테트리스 게임](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)

## 🚀 데모

게임을 바로 플레이해보세요! [Live Demo](https://your-username.github.io/tetris-game)

## ✨ 특징

### 🎯 핵심 기능
- **7가지 테트리스 블록**: I, O, T, S, Z, J, L 블록
- **완전한 게임 로직**: 블록 이동, 회전, 낙하, 충돌 감지
- **라인 클리어 시스템**: 가로줄이 가득 차면 자동으로 제거
- **점수 및 레벨 시스템**: 라인 클리어 시 점수 획득, 레벨업 시 속도 증가

### 🎮 조작법
- **← →**: 좌우 이동
- **↓**: 빠른 낙하
- **↑**: 블록 회전
- **스페이스바**: 즉시 낙하 (하드 드롭)

### 🎨 UI/UX
- **모던한 디자인**: 그라데이션과 글래스모피즘 효과
- **반응형 레이아웃**: PC, 태블릿, 모바일에서 완벽하게 작동
- **다음 블록 미리보기**: 다음에 나올 블록을 미리 확인 가능
- **실시간 점수 표시**: 점수, 레벨, 클리어한 라인 수 표시

## 🛠️ 기술 스택

- **HTML5**: 시맨틱 마크업, Canvas API
- **CSS3**: Flexbox, Grid, 애니메이션, 반응형 디자인
- **JavaScript ES6+**: 클래스, 모듈, 화살표 함수, 구조 분해

## 📁 프로젝트 구조

```
tetris-game/
├── index.html          # 메인 HTML 파일
├── style.css           # CSS 스타일시트
├── script.js           # JavaScript 게임 로직
├── PRD.md              # 제품 요구사항 문서
├── TRD.md              # 기술 요구사항 문서
├── README.md           # 프로젝트 설명서
└── .gitignore          # Git 무시 파일 목록
```

## 🚀 시작하기

### 설치 및 실행

1. **저장소 클론**
   ```bash
   git clone https://github.com/your-username/tetris-game.git
   cd tetris-game
   ```

2. **게임 실행**
   - `index.html` 파일을 웹 브라우저에서 열기
   - 또는 로컬 서버 실행:
     ```bash
     # Python 3
     python -m http.server 8000
     
     # Node.js (http-server 설치 필요)
     npx http-server
     ```

3. **게임 플레이**
   - "시작" 버튼을 클릭하여 게임 시작
   - 키보드 화살표 키로 블록 조작

## 🎮 게임 규칙

### 기본 규칙
- 블록이 화면 상단에서 떨어져 내려옵니다
- 블록을 좌우로 이동하거나 회전시킬 수 있습니다
- 블록이 바닥이나 다른 블록에 닿으면 고정됩니다
- 가로줄이 완전히 채워지면 해당 라인이 제거됩니다

### 점수 시스템
- **1줄 클리어**: 100 × 현재 레벨
- **2줄 클리어**: 300 × 현재 레벨
- **3줄 클리어**: 500 × 현재 레벨
- **4줄 클리어**: 800 × 현재 레벨

### 레벨 시스템
- 10줄을 클리어할 때마다 레벨이 증가합니다
- 레벨이 올라갈수록 블록 낙하 속도가 빨라집니다

## 🔧 개발

### 개발 환경 설정
```bash
# 저장소 클론
git clone https://github.com/your-username/tetris-game.git
cd tetris-game

# 브라우저에서 index.html 열기
open index.html
```

### 코드 구조
- `TetrisGame` 클래스: 메인 게임 엔진
- `TETRIS_PIECES`: 테트로미노 블록 정의
- Canvas 2D Context를 사용한 렌더링
- RequestAnimationFrame 기반 게임 루프

## 📱 브라우저 호환성

- ✅ Chrome 80+
- ✅ Firefox 75+
- ✅ Safari 13+
- ✅ Edge 80+

## 🤝 기여하기

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 라이선스

이 프로젝트는 MIT 라이선스 하에 배포됩니다. 자세한 내용은 `LICENSE` 파일을 참조하세요.

## 👨‍💻 개발자

**Your Name**
- GitHub: [@your-username](https://github.com/your-username)
- Email: your.email@example.com

## 🙏 감사의 말

- 테트리스 게임의 원작자 Alexey Pajitnov에게 감사드립니다
- 모든 오픈소스 커뮤니티에 감사드립니다

## 📞 문의

프로젝트에 대한 문의사항이 있으시면 이슈를 생성해 주세요.

---

⭐ 이 프로젝트가 도움이 되었다면 스타를 눌러주세요!
