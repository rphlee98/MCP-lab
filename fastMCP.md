### 프로젝트 구조: 가상환경을 생성하고 초기화하면 자동 생성 
```
C:\project\my_mcp
 ├── server.py
 ├── pyproject.toml
 ├── README.md
 └── ... (생략) ...

```
- 프로젝트: my_mcp
- 디렉토리: C:\project\my_mcp
- 스크립트: server.py

### 프로젝트 디렉토리로 이동 후 프로젝트 초기화 하기
```
c:
cd project

uv init my_mcp
```
### FastMCP 설치하기
```
cd my_mcp
uv add "mcp[cli]"
uv add fastmcp
```

### 설치 확인하기
```
fastmcp version
```

### 실행결과
```
'fastmcp'은(는) 내부 또는 외부 명령, 실행할 수 있는 프로그램, 또는 배치 파일이 아닙니다.
```
- fastmcp version 명령어가 인식되지 않는다는 오류가 발생했습니다. 이는 fastmcp가 설치된 .venv 가상 환경을 활성화하지 않고 명령어를 실행했기 때문입니다.

### 방법 1: uv를 사용하여 FastMCP 명령어를 실행
```
uv run fastmcp version
```

### 방법 2: .venv 가상환경을 활성화한 후 명령어 실행
```
.\.venv\Scripts\activate
fastmcp version
```

### 실행결과(다음과 같으면 성공)
```
FastMCP version:                                              2.8.1
MCP version:                                                  1.9.4
Python version:                                              3.13.5
Platform:                                                Windows...
FastMCP root path: D:\project\my-mcp-server\.venv\Lib\site-packages
```

### FastMCP 서버 객체 생성하기: FastMCP 서버를 구축하기 위해 server.py 파일을 생성하고 아래 코드 입력
```
from fastmcp import FastMCP
mcp = FastMCP("My MCP Server")

```

### 코드 설명
```
from fastmcp import FastMCP: FastMCP 모듈에서 FastMCP 클래스를 가져옵니다.
mcp = FastMCP("My MCP Server"): FastMCP 클래스의 인스턴스를 생성합니다.
이 인스턴스는 MCP 서버의 핵심 기능을 제공합니다.
"My MCP Server"는 서버의 이름으로, 원하는 이름으로 변경할 수 있습니다.

이 코드가 오류 없이 실행 되면 FastMCP 서버의 객체가 성공적으로 생성된 것입니다.
이제 이 객체에 도구(tool)를 추가하고, 서버를 실행하는 작업을 진행할 수 있습니다.
```

### 2개의 수를 곱하는 도구 예제

```
from fastmcp import FastMCP

mcp = FastMCP(name="calculator")

@mcp.tool
def multiply(a: float, b: float) -> float:
    """Multiplies two numbers together."""
    return a * b

if __name__ == "__main__":
    mcp.run()
```

### 서버 실행하기: 터미널을 열고 아래 명령어를 입력하여 서버 실행
```
uv run server.py
```

### 실행결과(성공)
```
INFO     Starting MCP server 'calculator' with transport 'stdio'               server.py:1168
```

### MCP 서버 등록: Claude Desktop의 claude_desktop_config.json
```
{
  "mcpServers": {
    "calculator": {
      "command": "uv",
      "args": [
        "--directory",
        "C:\\project\\my_mcp",
        "run",
        "server.py"
      ]
    }
  }
}

```

### MCP 서버 등록: Cursor의 mcp.json
```
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "C:\\Users\\user\\Documents"
      ]
    },
    "context7-mcp": {
      "command": "cmd",
      "args": [
        "/c",
        "npx",
        "-y",
        "@smithery/cli@latest",
        "run",
        "@upstash/context7-mcp",
        "--key",
        "Smithery Key"
      ]
    },
    "calculator": {
      "command": "uv",
      "args": [
        "--directory",
        "C:\\project\\my-mcp",
        "run",
        "server.py"
      ]
    }
  }
}
```

### 등록확인(Cursor)
- 수정한 mcp.json 파일을 저장하고 Cursor Settings 탭으로 이동
- MCP Tool에서 Calculator 가 추가된 것을 확인
- 우측의 토글 스위치를 두번 클릭하여 연결을 활성화
-  새 채팅을 시작하고 다으과 같은 질의 요청 "7 곱하기 8은?"
