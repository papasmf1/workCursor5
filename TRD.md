# 테트리스 게임 TRD (Technical Requirements Document)

## 1. 문서 개요

### 1.1 문서 목적
이 문서는 웹 기반 테트리스 게임의 기술적 요구사항과 구현 세부사항을 정의합니다. 개발팀이 일관된 기술 표준을 따라 게임을 구현할 수 있도록 가이드라인을 제공합니다.

### 1.2 문서 범위
- 기술 아키텍처 및 설계 패턴
- 개발 환경 및 도구
- 코드 구조 및 모듈화
- 성능 최적화 방안
- 테스트 전략
- 배포 및 유지보수

### 1.3 대상 독자
- 프론트엔드 개발자
- 웹 개발자
- 품질 보증 엔지니어
- 프로젝트 매니저

## 2. 기술 아키텍처

### 2.1 전체 아키텍처

```
┌─────────────────────────────────────────┐
│                Browser                  │
├─────────────────────────────────────────┤
│  HTML5 Canvas API                       │
├─────────────────────────────────────────┤
│  JavaScript Game Engine                 │
│  ┌─────────────────────────────────────┐│
│  │  Game Loop                          ││
│  │  ┌─────────────────────────────────┐││
│  │  │  Input Handler                  │││
│  │  │  Physics Engine                 │││
│  │  │  Render Engine                  │││
│  │  │  Audio Engine                   │││
│  │  └─────────────────────────────────┘││
│  └─────────────────────────────────────┘│
├─────────────────────────────────────────┤
│  CSS3 Styling & Animation              │
├─────────────────────────────────────────┤
│  HTML5 Structure                       │
└─────────────────────────────────────────┘
```

### 2.2 기술 스택

#### 2.2.1 프론트엔드
- **HTML5**: 시맨틱 마크업, Canvas API
- **CSS3**: Flexbox, Grid, 애니메이션, 반응형 디자인
- **JavaScript ES6+**: 클래스, 모듈, 화살표 함수, 구조 분해

#### 2.2.2 브라우저 API
- **Canvas 2D Context**: 게임 렌더링
- **RequestAnimationFrame**: 게임 루프
- **KeyboardEvent**: 사용자 입력 처리
- **LocalStorage**: 게임 데이터 저장 (선택사항)

## 3. 개발 환경

### 3.1 필수 도구
- **코드 에디터**: VS Code, WebStorm, 또는 동등한 IDE
- **브라우저**: Chrome DevTools, Firefox Developer Tools
- **버전 관리**: Git
- **패키지 매니저**: npm 또는 yarn (선택사항)

### 3.2 개발 환경 설정
```bash
# 프로젝트 구조
tetris-game/
├── index.html          # 메인 HTML 파일
├── style.css           # CSS 스타일시트
├── script.js           # JavaScript 게임 로직
├── assets/             # 이미지, 사운드 파일 (선택사항)
├── tests/              # 테스트 파일
└── docs/               # 문서 파일
```

### 3.3 브라우저 호환성
- **Chrome**: 80+ (Canvas 2D Context 지원)
- **Firefox**: 75+ (ES6+ 지원)
- **Safari**: 13+ (Canvas 2D Context 지원)
- **Edge**: 80+ (Chromium 기반)

## 4. 코드 구조 및 설계

### 4.1 클래스 구조

```javascript
class TetrisGame {
    // 게임 상태 관리
    constructor()
    initEventListeners()
    
    // 게임 루프
    gameLoop()
    update()
    render()
    
    // 블록 관리
    spawnNewPiece()
    movePiece()
    rotatePiece()
    placePiece()
    
    // 충돌 감지
    checkCollision()
    
    // 게임 로직
    clearLines()
    updateScore()
    gameOver()
    
    // 렌더링
    draw()
    drawBoard()
    drawPiece()
    drawNextPiece()
}
```

### 4.2 모듈화 전략

#### 4.2.1 게임 엔진 모듈
```javascript
// 게임 상태 관리
const GameState = {
    RUNNING: 'running',
    PAUSED: 'paused',
    GAME_OVER: 'game_over'
};

// 게임 설정
const GameConfig = {
    BOARD_WIDTH: 10,
    BOARD_HEIGHT: 20,
    BLOCK_SIZE: 30,
    DROP_INTERVAL: 1000
};
```

#### 4.2.2 블록 관리 모듈
```javascript
// 테트로미노 정의
const TETRIS_PIECES = [
    { shape: [...], color: '...' },
    // ... 다른 블록들
];

// 블록 회전 로직
function rotateMatrix(matrix) { ... }
```

### 4.3 데이터 구조

#### 4.3.1 게임 보드
```javascript
// 2D 배열로 게임 보드 표현
const board = Array(BOARD_HEIGHT).fill()
    .map(() => Array(BOARD_WIDTH).fill(0));

// 0: 빈 공간, 문자열: 블록 색상
```

#### 4.3.2 블록 객체
```javascript
const piece = {
    shape: [[1, 1, 1, 1]],  // 블록 모양
    color: '#00f5ff',        // 블록 색상
    x: 3,                    // X 좌표
    y: 0                     // Y 좌표
};
```

## 5. 성능 최적화

### 5.1 렌더링 최적화

#### 5.1.1 Canvas 최적화
```javascript
// 더블 버퍼링 사용
const offscreenCanvas = document.createElement('canvas');
const offscreenCtx = offscreenCanvas.getContext('2d');

// 불필요한 리페인트 방지
function draw() {
    // 변경된 부분만 다시 그리기
    if (needsRedraw) {
        clearCanvas();
        drawBoard();
        drawCurrentPiece();
        needsRedraw = false;
    }
}
```

#### 5.1.2 게임 루프 최적화
```javascript
// RequestAnimationFrame 사용
function gameLoop() {
    const currentTime = performance.now();
    const deltaTime = currentTime - lastTime;
    
    if (deltaTime >= dropInterval) {
        update();
        lastTime = currentTime;
    }
    
    render();
    requestAnimationFrame(gameLoop);
}
```

### 5.2 메모리 관리

#### 5.2.1 객체 풀링
```javascript
// 블록 객체 재사용
class PiecePool {
    constructor() {
        this.pool = [];
    }
    
    getPiece() {
        return this.pool.pop() || this.createPiece();
    }
    
    returnPiece(piece) {
        this.pool.push(piece);
    }
}
```

#### 5.2.2 가비지 컬렉션 최적화
```javascript
// 불필요한 객체 생성 방지
const tempVector = { x: 0, y: 0 };

function checkCollision(piece, dx, dy) {
    tempVector.x = piece.x + dx;
    tempVector.y = piece.y + dy;
    // ... 충돌 검사
}
```

## 6. 입력 처리

### 6.1 키보드 입력

#### 6.1.1 이벤트 리스너
```javascript
document.addEventListener('keydown', (e) => {
    if (!gameRunning || gamePaused) return;
    
    switch(e.code) {
        case 'ArrowLeft':
            movePiece(-1, 0);
            break;
        case 'ArrowRight':
            movePiece(1, 0);
            break;
        // ... 다른 키들
    }
});
```

#### 6.1.2 키 반복 방지
```javascript
let keyPressed = {};

document.addEventListener('keydown', (e) => {
    if (keyPressed[e.code]) return;
    keyPressed[e.code] = true;
    // ... 키 처리
});

document.addEventListener('keyup', (e) => {
    keyPressed[e.code] = false;
});
```

### 6.2 터치 입력 (모바일)

#### 6.2.1 터치 이벤트
```javascript
// 모바일 터치 지원
canvas.addEventListener('touchstart', handleTouch);
canvas.addEventListener('touchmove', handleTouch);
canvas.addEventListener('touchend', handleTouch);

function handleTouch(e) {
    e.preventDefault();
    const touch = e.touches[0];
    const rect = canvas.getBoundingClientRect();
    const x = touch.clientX - rect.left;
    const y = touch.clientY - rect.top;
    // ... 터치 좌표 처리
}
```

## 7. 테스트 전략

### 7.1 단위 테스트

#### 7.1.1 게임 로직 테스트
```javascript
// Jest 또는 Mocha 사용
describe('TetrisGame', () => {
    test('블록 회전 테스트', () => {
        const piece = { shape: [[1, 1], [1, 0]] };
        const rotated = rotateMatrix(piece.shape);
        expect(rotated).toEqual([[1, 1], [0, 1]]);
    });
    
    test('충돌 감지 테스트', () => {
        const piece = { x: 0, y: 0, shape: [[1, 1]] };
        expect(checkCollision(piece, -1, 0)).toBe(true);
    });
});
```

#### 7.1.2 점수 계산 테스트
```javascript
test('라인 클리어 점수 계산', () => {
    const linesCleared = 2;
    const level = 3;
    const score = calculateScore(linesCleared, level);
    expect(score).toBe(600); // 2 * 100 * 3
});
```

### 7.2 통합 테스트

#### 7.2.1 게임 플로우 테스트
```javascript
test('완전한 게임 플로우', () => {
    const game = new TetrisGame();
    game.startGame();
    
    // 블록 생성 테스트
    expect(game.currentPiece).toBeDefined();
    
    // 블록 이동 테스트
    game.movePiece(1, 0);
    expect(game.currentPiece.x).toBe(1);
    
    // 게임 오버 테스트
    // ... 게임 오버 조건 테스트
});
```

### 7.3 성능 테스트

#### 7.3.1 프레임레이트 테스트
```javascript
test('60fps 유지 테스트', () => {
    const game = new TetrisGame();
    const startTime = performance.now();
    let frameCount = 0;
    
    function testLoop() {
        frameCount++;
        if (performance.now() - startTime < 1000) {
            requestAnimationFrame(testLoop);
        } else {
            expect(frameCount).toBeGreaterThan(55); // 60fps 근사치
        }
    }
    
    testLoop();
});
```

## 8. 오류 처리

### 8.1 예외 처리

#### 8.1.1 게임 상태 오류
```javascript
function startGame() {
    try {
        if (gameRunning) {
            throw new Error('게임이 이미 실행 중입니다.');
        }
        // ... 게임 시작 로직
    } catch (error) {
        console.error('게임 시작 오류:', error);
        showErrorMessage('게임을 시작할 수 없습니다.');
    }
}
```

#### 8.1.2 렌더링 오류
```javascript
function draw() {
    try {
        clearCanvas();
        drawBoard();
        drawCurrentPiece();
    } catch (error) {
        console.error('렌더링 오류:', error);
        // 기본 화면으로 복구
        drawErrorScreen();
    }
}
```

### 8.2 사용자 피드백

#### 8.2.1 오류 메시지
```javascript
function showErrorMessage(message) {
    const errorDiv = document.createElement('div');
    errorDiv.className = 'error-message';
    errorDiv.textContent = message;
    document.body.appendChild(errorDiv);
    
    setTimeout(() => {
        errorDiv.remove();
    }, 3000);
}
```

## 9. 배포 및 유지보수

### 9.1 빌드 프로세스

#### 9.1.1 파일 최적화
```bash
# CSS 압축
npx clean-css-cli -o style.min.css style.css

# JavaScript 압축
npx terser script.js -o script.min.js

# HTML 최적화
npx html-minifier-terser index.html -o index.min.html
```

#### 9.1.2 배포 체크리스트
- [ ] 모든 파일이 올바른 인코딩으로 저장됨
- [ ] 상대 경로가 올바르게 설정됨
- [ ] 브라우저 호환성 테스트 완료
- [ ] 성능 테스트 통과
- [ ] 모바일 반응형 테스트 완료

### 9.2 모니터링

#### 9.2.1 성능 모니터링
```javascript
// 성능 메트릭 수집
const performanceMetrics = {
    frameRate: 0,
    memoryUsage: 0,
    renderTime: 0
};

function collectMetrics() {
    performanceMetrics.frameRate = calculateFrameRate();
    performanceMetrics.memoryUsage = performance.memory?.usedJSHeapSize || 0;
    performanceMetrics.renderTime = measureRenderTime();
}
```

#### 9.2.2 오류 로깅
```javascript
// 오류 로깅 시스템
function logError(error, context) {
    const errorLog = {
        timestamp: new Date().toISOString(),
        error: error.message,
        stack: error.stack,
        context: context,
        userAgent: navigator.userAgent
    };
    
    // 로컬 스토리지에 저장 또는 서버로 전송
    localStorage.setItem('errorLog', JSON.stringify(errorLog));
}
```

## 10. 보안 고려사항

### 10.1 클라이언트 사이드 보안

#### 10.1.1 입력 검증
```javascript
function validateInput(input) {
    // XSS 방지를 위한 입력 검증
    if (typeof input !== 'string') return false;
    if (input.length > 100) return false;
    return /^[a-zA-Z0-9\s]*$/.test(input);
}
```

#### 10.1.2 데이터 무결성
```javascript
function validateGameState(gameState) {
    // 게임 상태 검증
    if (!gameState || typeof gameState !== 'object') return false;
    if (gameState.score < 0 || gameState.level < 1) return false;
    return true;
}
```

## 11. 확장성 고려사항

### 11.1 모듈화 설계

#### 11.1.1 플러그인 시스템
```javascript
// 게임 플러그인 인터페이스
class GamePlugin {
    constructor(game) {
        this.game = game;
    }
    
    init() { }
    update() { }
    render() { }
    destroy() { }
}

// 사운드 플러그인 예시
class SoundPlugin extends GamePlugin {
    init() {
        this.audioContext = new AudioContext();
    }
    
    playSound(soundType) {
        // 사운드 재생 로직
    }
}
```

### 11.2 설정 시스템

#### 11.2.1 게임 설정
```javascript
const GameSettings = {
    graphics: {
        blockSize: 30,
        showGrid: true,
        animations: true
    },
    audio: {
        enabled: true,
        volume: 0.7
    },
    controls: {
        keyBindings: {
            moveLeft: 'ArrowLeft',
            moveRight: 'ArrowRight',
            rotate: 'ArrowUp',
            drop: 'ArrowDown',
            hardDrop: 'Space'
        }
    }
};
```

## 12. 결론

이 TRD는 테트리스 게임의 기술적 구현을 위한 종합적인 가이드라인을 제공합니다. 개발팀은 이 문서를 참고하여 일관된 코드 품질과 성능을 유지하면서 확장 가능한 게임을 구현할 수 있습니다.

### 12.1 핵심 원칙
- **성능 우선**: 60fps 유지 및 메모리 효율성
- **코드 품질**: 모듈화된 구조와 명확한 네이밍
- **사용자 경험**: 부드러운 애니메이션과 즉각적인 반응
- **확장성**: 향후 기능 추가를 고려한 설계

### 12.2 다음 단계
1. 개발 환경 설정 및 프로젝트 초기화
2. 핵심 게임 엔진 구현
3. UI/UX 컴포넌트 개발
4. 테스트 코드 작성 및 실행
5. 성능 최적화 및 배포
