### 1일차 오후

### MCP 실습환경 설정

##### 워밍업 예제

### 9-1. MCP.tool 실습 (tutorial_1.py) 

```python
from mcp.server.fastmcp import FastMCP

#MCP 서버 생성하기

mcp = FastMCP(name=“tutorial_1”) 1

@mcp.tool()

def echo(message: str) -> str:

# 입력받은 메시지를 그대로 반환하는 도구

  return message + “ 라는 메시지가 입력되었습니다. 프로그램의 시작은 언제나 Hello World!”

# 서버 실행하기

if __name__ == “__main__”:

mcp.run()

```


- 설정 파일 작성하기
	- claude_desktop_config.json 파일 작성, 위치는 C:\Users\user\AppData\Roaming\Claude 폴더에 위치
```
{

“mcpServers”: {

     “tutorial_1”: {

          “command”: “python”,

          “args”: [

                   “c:\\MCP_lab\\tutorial_1.py”

            ]

        }

    }

}
```

### 9-2 MCP.resource 실습(tutorial_2.py)

```
from mcp.server.fastmcp import FastMCP

#MCP 서버 생성하기

mcp = FastMCP(name=“tutorial_2”)

@mcp.tool()

def add(a: int, b: int) -> int:

``` 두 숫자를 더하는 함수 ```

  return a + b

@mcp.resource(“greeting://hello”)

def get_greeting() -> str:

```인사말을 제공하는 함수```

  return f“Hello, world!”

# 서버 실행하기

if __name__ == “__main__”:

mcp.run()
```

### 설정파일

```
{

  “mcpServers”: {
    “tutorial_1”: {
        “command”: “python”,
        “args”: [
          “c:\\MCP_lab\\tutorial_1.py”
        ]
    },
   “tutorial_2”: {
       “command”: “python”,
       “args”: [
         “c:\\MCP_lab\\tutorial_2.py”
       ]
    }
   }
}
```

### 9-3 MCP.prompt 실습(tutorial_3.py)

```
from mcp.server.fastmcp import FastMCP

#MCP 서버 생성하기

mcp = FastMCP(name=“tutorial_3”)

# Prompt 확장 예제

@mcp.prompt()

def prompt_extension(contents: str) -> str:

# 프롬프트에서 사실과 의견을 구분합니다. 

  return f"""{contents}

이 프롬프트에 대해 아래와 같은 템플릿 서식에 맞도록 답변해줘.

* 사실:

* 의견:

"""


# 서버 실행하기

if __name__ == “__main__”:

mcp.run()
```

### 설정파일

```
{

  “mcpServers”: {
      “tutorial_1”: {
          “command”: “python”,
          “args”: [
             “c:\\MCP_lab\\tutorial_1.py”
          ]
      },
      “tutorial_2”: {
          “command”: “python”,
          “args”: [
             “c:\\MCP_lab\\tutorial_2.py”
          ]
      },
      “tutorial_3”: {
          “command”: “python”,
          “args”: [
             “c:\\MCP_lab\\tutorial_3.py”
          ]
       }
   }
}
```



## 실습 2

**2) 폴더 생성, 삭제, 목록 조회하기

- 아래 코드를 vs code에서 작성하고 test.py 으로 저장
```
import os

# 현재 폴더에 temp 폴더 생성

os.makedirs(“c:/test/temp”, exist_ok=True)

# 현재 폴더에 temp 폴더 삭제

# os.rmdir(“c:/test/temp”)
```

다음 코드를 작성하고 server.py 으로 저장

```
from mcp.server.fastmcp import FastMCP

# MCP 서버 생성
mcp = FastMCP(name="server")


@mcp.tool()
def create_folder(folder_name: str) -> str:
    """
    c:/test/ 아래 폴더를 생성합니다.

    Parameters
    ----------
    folder_name : str
        생성할 폴더 이름

    Returns
    -------
    str
        생성 결과 메시지
    """
    import os

    folder_path = os.path.join("c:/test", folder_name)
    if not os.path.exists(folder_path):
        os.makedirs(folder_path)
        return f"폴더 '{folder_name}' 가 생성되었습니다."
    else:
        return f"폴더 '{folder_name}' 는 이미 존재합니다."


@mcp.tool()
def delete_folder(folder_name: str) -> str:
    """
    c:/test/ 아래 폴더를 삭제합니다.

    Parameters
    ----------
    folder_name : str
        삭제할 폴더 이름

    Returns
    -------
    str
        삭제 결과 메시지
    """
    import os

    folder_path = os.path.join("c:/test", folder_name)
    if os.path.exists(folder_path):
        os.rmdir(folder_path)
        return f"폴더 '{folder_name}' 가 삭제되었습니다."
    else:
        return f"폴더 '{folder_name}' 는 존재하지 않습니다."


@mcp.tool()
def list_folders() -> list:
    """
    c:/test/ 아래 폴더 목록을 반환합니다.

    Returns
    -------
    list
        폴더 목록
    """
    import os

    folder_path = "c:/test"
    folders = [
        f
        for f in os.listdir(folder_path)
        if os.path.isdir(os.path.join(folder_path, f))
    ]
    return folders


# 서버 실행
if __name__ == "__main__":
    mcp.run()
```


**3) 파일 입출력 실습

```
from mcp.server.fastmcp import FastMCP

# MCP 서버 생성
mcp = FastMCP(name="server")


@mcp.tool()
def create_folder(folder_name: str) -> str:
    """
    c:/test/ 아래 폴더를 생성합니다.

    Parameters
    ----------
    folder_name : str
        생성할 폴더 이름

    Returns
    -------
    str
        생성 결과 메시지
    """
    import os

    folder_path = os.path.join("c:/test", folder_name)
    if not os.path.exists(folder_path):
        os.makedirs(folder_path)
        return f"폴더 '{folder_name}' 가 생성되었습니다."
    else:
        return f"폴더 '{folder_name}' 는 이미 존재합니다."


@mcp.tool()
def delete_folder(folder_name: str) -> str:
    """
    c:/test/ 아래 폴더를 삭제합니다.

    Parameters
    ----------
    folder_name : str
        삭제할 폴더 이름

    Returns
    -------
    str
        삭제 결과 메시지
    """
    import os

    folder_path = os.path.join("c:/test", folder_name)
    if os.path.exists(folder_path):
        os.rmdir(folder_path)
        return f"폴더 '{folder_name}' 가 삭제되었습니다."
    else:
        return f"폴더 '{folder_name}' 는 존재하지 않습니다."


@mcp.tool()
def list_folders() -> list:
    """
    c:/test/ 아래 폴더 목록을 반환합니다.

    Returns
    -------
    list
        폴더 목록
    """
    import os

    folder_path = "c:/test"
    folders = [
        f
        for f in os.listdir(folder_path)
        if os.path.isdir(os.path.join(folder_path, f))
    ]
    return folders


@mcp.tool()
def write_file(file_name: str, content: str) -> str:
    """
    c:/test/ 아래에 파일을 생성하고 내용을 작성합니다.

    Parameters
    ----------
    file_name : str
        생성할 파일 이름 (확장자 포함)
    content : str
        파일에 작성할 내용

    Returns
    -------
    str
        파일 작성 결과 메시지
    """
    import os

    file_path = os.path.join("c:/test", file_name)
    try:
        with open(file_path, "w", encoding="utf-8") as f:
            f.write(content)
        return f"파일 '{file_name}'에 내용이 성공적으로 작성되었습니다."
    except Exception as e:
        return f"파일 작성 중 오류가 발생했습니다: {str(e)}"


@mcp.tool()
def read_file(file_name: str) -> str:
    """
    c:/test/ 아래의 파일 내용을 읽어옵니다.

    Parameters
    ----------
    file_name : str
        읽을 파일 이름 (확장자 포함)

    Returns
    -------
    str
        파일 내용 또는 오류 메시지
    """
    import os

    file_path = os.path.join("c:/test", file_name)
    if not os.path.exists(file_path):
        return f"파일 '{file_name}'이 존재하지 않습니다."

    try:
        with open(file_path, "r", encoding="utf-8") as f:
            content = f.read()
        return content
    except Exception as e:
        return f"파일 읽기 중 오류가 발생했습니다: {str(e)}"


@mcp.tool()
def append_to_file(file_name: str, content: str) -> str:
    """
    c:/test/ 아래의 파일에 내용을 추가합니다.

    Parameters
    ----------
    file_name : str
        내용을 추가할 파일 이름 (확장자 포함)
    content : str
        추가할 내용

    Returns
    -------
    str
        파일 추가 결과 메시지
    """
    import os

    file_path = os.path.join("c:/test", file_name)
    if not os.path.exists(file_path):
        return f"파일 '{file_name}'이 존재하지 않습니다."

    try:
        with open(file_path, "a", encoding="utf-8") as f:
            f.write(content)
        return f"파일 '{file_name}'에 내용이 성공적으로 추가되었습니다."
    except Exception as e:
        return f"파일 내용 추가 중 오류가 발생했습니다: {str(e)}"


@mcp.tool()
def list_files() -> list:
    """
    c:/test/ 아래 파일 목록을 반환합니다.

    Returns
    -------
    list
        파일 목록
    """
    import os

    folder_path = "c:/test"
    files = [
        f
        for f in os.listdir(folder_path)
        if os.path.isfile(os.path.join(folder_path, f))
    ]
    return files


# 서버 실행
if __name__ == "__main__":
    mcp.run()
```

### [응용예제 1] 책 예제 폴더 만들기

- 클로드가 생성해 준 코드 예

```
import os

from mcp.server.fastmcp import FastMCP
# MCP 서버 초기화

mcp = FastMCP("Book Structure Generator")
BASE_PATH = "C:/test"

@mcp.tool()

def create_book_structure(
  part_count: int,
  chapters_per_part: list[int],
  base_folder: str = "book_project"
) -> str:

"""
프로그래밍 책의 폴더 구조를 자동으로 생성합니다.
Parameters
----------
part_count : int
  생성할 파트의 수
chapters_per_part : list[int]
  각 파트당 챕터 수 리스트 (예: [8, 7, 5])
base_folder : str
  기본 프로젝트 폴더명 (기본값: "book_project")
Returns
-------
str
  생성 결과 메시지
"""
if len(chapters_per_part) != part_count:
    return f"오류: part_count({part_count})와 chapters_per_part 리스트 길이({len(chapters_per_part)})가 일치하지 않습니다."
    project_path = os.path.join(BASE_PATH, base_folder)

# 프로젝트 기본 폴더 생성
if not os.path.exists(project_path):
  os.makedirs(project_path)
  chapter_number = 1
  created_folders = []

for part_idx in range(1, part_count + 1):
  part_folder = f"PART{part_idx:02d}"
  part_path = os.path.join(project_path, part_folder)
  os.makedirs(part_path, exist_ok=True)
  created_folders.append(part_folder)
  # 해당 파트의 챕터 수만큼 반복
  for _ in range(chapters_per_part[part_idx - 1]):
    chapter_folder = f"CHAPTER{chapter_number:02d}"
    chapter_path = os.path.join(part_path, chapter_folder)
    os.makedirs(chapter_path, exist_ok=True)
    created_folders.append(f" {chapter_folder}")

# 각 챕터 내 하위 폴더 생성
    subfolders = ["code_examples", "screenshots", "resources"]
    for subfolder in subfolders:
       subfolder_path = os.path.join(chapter_path, subfolder)
       os.makedirs(subfolder_path, exist_ok=True)
       created_folders.append(f" {subfolder}")
    chapter_number += 1

result_message = f"✅ 책 구조가 성공적으로 생성되었습니다!\n\n"
result_message += f"프로젝트 경로: {project_path}\n"
result_message += f"총 파트 수: {part_count}\n"
result_message += f"총 챕터 수: {chapter_number - 1}\n\n"
result_message += "생성된 폴더 구조:\n"
result_message += "\n".join(created_folders)

return result_message

@mcp.tool()

def create_simple_book_structure(
   part_count: int,
   chapters_per_part: int,
   base_folder: str = "book_project"
) -> str:
"""
모든 파트에 동일한 챕터 수를 가진 책 구조를 생성합니다.
Parameters
----------
part_count : int
   생성할 파트의 수
chapters_per_part : int
   각 파트당 챕터 수 (모든 파트 동일)
base_folder : str
   기본 프로젝트 폴더명 (기본값: "book_project")
Returns
-------
str
  생성 결과 메시지
"""

  chapters_list = [chapters_per_part] * part_count
  return create_book_structure(part_count, chapters_list, base_folder)

if __name__ == "__main__":
   mcp.run()
```


## [응용예제 2] 다운로드 폴더 정리하기 

- 클로드가 생성해 준 다운로드 폴더 정리 MCP서버 코드

```
from mcp.server.fastmcp import FastMCP

import os

import shutil

from datetime import datetime

from pathlib import Path

# Create a MCP server

mcp = FastMCP("download_organizer")

# 파일 확장자별 카테고리 매핑

FILE_CATEGORIES = {

'images': ['.jpg', '.jpeg', '.png', '.gif', '.bmp', '.svg', '.webp', '.ico'],

'documents': ['.pdf', '.doc', '.docx', '.xls', '.xlsx', '.ppt', '.pptx', '.txt', '.hwp'],

'videos': ['.mp4', '.avi', '.mkv', '.mov', '.wmv', '.flv', '.webm', '.mpeg'],

'archives': ['.zip', '.rar', '.7z', '.tar', '.gz', '.bz2', '.xz'],

'others': [] # 기타 파일들

}

@mcp.tool()

def organize_downloads(downloads_path: str = None) -> str:

"""

다운로드 폴더의 파일들을 유형별로 분류하여 정리합니다.

파일은 삭제되지 않고 해당 카테고리 폴더로 이동됩니다.

Parameters

----------

downloads_path : str, optional

정리할 다운로드 폴더 경로 (기본값: 사용자의 다운로드 폴더)

Returns

-------

str

정리 결과 메시지

"""

# 다운로드 폴더 경로 설정

if downloads_path is None:

downloads_path = str(Path.home() / "Downloads")

if not os.path.exists(downloads_path):

return f"다운로드 폴더를 찾을 수 없습니다: {downloads_path}"

# 카테고리별 폴더 생성

for category in FILE_CATEGORIES.keys():

category_path = os.path.join(downloads_path, category)

os.makedirs(category_path, exist_ok=True)

# 파일 정리 통계

moved_files = {category: 0 for category in FILE_CATEGORIES.keys()}

# 다운로드 폴더의 모든 파일 처리

for item in os.listdir(downloads_path):

item_path = os.path.join(downloads_path, item)

# 폴더는 건너뛰기

if os.path.isdir(item_path):

continue

# 파일 확장자 확인

file_ext = os.path.splitext(item)[1].lower()

# 파일이 속할 카테고리 찾기

target_category = 'others'

for category, extensions in FILE_CATEGORIES.items():

if file_ext in extensions:

target_category = category

break

# 파일 이동

try:

# 파일 수정 날짜 가져오기

file_mtime = os.path.getmtime(item_path)

file_date = datetime.fromtimestamp(file_mtime).strftime('%Y%m%d')

# 새 파일명 생성 (날짜_원본파일명)

new_filename = f"{file_date}_{item}"

target_path = os.path.join(downloads_path, target_category, new_filename)

# 동일한 파일명이 있으면 숫자 추가

counter = 1

while os.path.exists(target_path):

name, ext = os.path.splitext(item)

new_filename = f"{file_date}_{name}_{counter}{ext}"

target_path = os.path.join(downloads_path, target_category, new_filename)

counter += 1

# 파일 이동

shutil.move(item_path, target_path)

moved_files[target_category] += 1

except Exception as e:

print(f"파일 이동 실패: {item} - {str(e)}")

# 결과 메시지 생성

result_lines = ["✅ 다운로드 폴더 정리 완료!\n"]

total_moved = 0

for category, count in moved_files.items():

if count > 0:

result_lines.append(f" 📁 {category}: {count}개 파일")

total_moved += count

result_lines.append(f"\n총 {total_moved}개의 파일이 정리되었습니다.")

return "\n".join(result_lines)

@mcp.tool()

def organize_custom_folder(folder_path: str) -> str:

"""

지정한 폴더의 파일들을 유형별로 분류하여 정리합니다.

Parameters

----------

folder_path : str

정리할 폴더의 전체 경로

Returns

-------

str

정리 결과 메시지

"""

return organize_downloads(folder_path)

@mcp.tool()

def preview_organization(downloads_path: str = None) -> str:

"""

파일을 실제로 이동하지 않고 어떻게 정리될지 미리 보여줍니다.

Parameters

----------

downloads_path : str, optional

확인할 다운로드 폴더 경로 (기본값: 사용자의 다운로드 폴더)

Returns

-------

str

정리 미리보기 결과

"""

# 다운로드 폴더 경로 설정

if downloads_path is None:

downloads_path = str(Path.home() / "Downloads")

if not os.path.exists(downloads_path):

return f"다운로드 폴더를 찾을 수 없습니다: {downloads_path}"

# 파일 분류 미리보기

preview = {category: [] for category in FILE_CATEGORIES.keys()}

for item in os.listdir(downloads_path):

item_path = os.path.join(downloads_path, item)

# 폴더는 건너뛰기

if os.path.isdir(item_path):

continue

# 파일 확장자 확인

file_ext = os.path.splitext(item)[1].lower()

# 파일이 속할 카테고리 찾기

target_category = 'others'

for category, extensions in FILE_CATEGORIES.items():

if file_ext in extensions:

target_category = category

break

preview[target_category].append(item)

# 결과 메시지 생성

result_lines = ["📋 다운로드 폴더 정리 미리보기\n"]

total_files = 0

for category, files in preview.items():

if files:

result_lines.append(f"\n📁 {category} ({len(files)}개):")

for file in files[:5]: # 최대 5개만 표시

result_lines.append(f" - {file}")

if len(files) > 5:

result_lines.append(f" ... 외 {len(files) - 5}개")

total_files += len(files)

result_lines.append(f"\n총 {total_files}개의 파일이 정리될 예정입니다.")

return "\n".join(result_lines)

# 서버 실행하기

if __name__ == "__main__":

   mcp.run()
```

## 2일차 실습예제 
### 1 웹데이터 가져오기

- 다음 코드를 crawler_1.py 으로 저장하고 python crawler_1.py 실행

```
import requests

from bs4 import BeautifulSoup

import pandas as pd

# 위니브 책 정보 페이지 URL

url = "https://paullab.co.kr/bookservice/"

# 웹 페이지 가져오기

response = requests.get(url)

soup = BeautifulSoup(response.text, "html.parser")

result = []

for book in soup.select(".book_name"):

   result.append(book.text.strip())

print(result)
```

- 다음 코드를 crawler_2.py 으로 작성하여 저장하고 python crawler_2.py 으로 실행
```
import pandas as pd

# 위니브 주요 주가지수 서비스

url = "https://paullab.co.kr/stock.html"

df = pd.read_html(url)

print(df[3])
```

## [MCP 서버 실습] 웹 크롤링

- server.py 파일에 다음 내용 추가

```
from mcp.server.fastmcp import FastMCP

# MCP 서버 생성

mcp = FastMCP(name="server")

# 기존 함수들 (파일 관련 함수들)

# 여기에 웹 크롤링 관련 함수 추가

@mcp.tool()

def crawl_url_return_book_name(url: str) -> str:

"""

URL을 입력 받아 해당 URL의 책 제목을 크롤링하여 반환합니다. 각 데이터는 콤마로 연결됩니다. 따라서 사용자에게 보여줄 때에는 콤마를 개행하여 보여주세요.

  Parameters

    ----------

    url : str

    크롤링할 웹 페이지 URL

    Returns

    -------

    str

    콤마로 구분된 책 제목 목록

    """

    import requests

    from bs4 import BeautifulSoup

    response = requests.get(url)

    soup = BeautifulSoup(response.text, "html.parser")

    result = []

    for book in soup.select(".book_name"):

    result.append(book.text.strip())

    return ", ".join(result)
```

### [응용 예제 3] 도서 정보 자동 수집 및 마크다운 리포트 작성

- 클로드에서 생성한 코드 예시

```
from mcp.server.fastmcp import FastMCP

import requests

from bs4 import BeautifulSoup

from datetime import datetime

import os

from pathlib import Path

# MCP 서버 생성하기

mcp = FastMCP(name="book_collector")

@mcp.tool()

def collect_book_info() -> str:

"""

위니브 책 정보 페이지에서 도서 정보를 수집하여 마크다운 파일로 저장하는 도구입니다.

기능:

- 위니브 책 정보 페이지에서 데이터 크롤링

- 책 제목, 가격, 저자, 설명 추출

- 마크다운 형식으로 정렬

- c:/reports/ 폴더에 저장

반환:

- 저장된 파일 경로 및 수집 결과

"""

try:

# URL에서 HTML 데이터 가져오기

url = "https://paullab.co.kr/bookservice/"

headers = {

'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'

}

response = requests.get(url, headers=headers, timeout=10)

response.raise_for_status()

# BeautifulSoup으로 HTML 파싱

soup = BeautifulSoup(response.content, 'html.parser')

# 책 정보 수집

books = []

book_rows = soup.find_all('div', class_='row')

for row in book_rows:

try:

# 책 제목 추출

title_elem = row.find('h2', class_='book_name')

if not title_elem:

continue

title = title_elem.get_text(strip=True)

# 책 정보 추출 (가격, 저자, 설명)

book_info_elems = row.find_all('p', class_='book_info')

price = ""

author = ""

description = ""

for elem in book_info_elems:

text = elem.get_text(strip=True)

if "가격:" in text:

price = text

elif "저자:" in text:

author = text

# 마지막 p 태그에서 설명 추출

if book_info_elems:

last_info = book_info_elems[-1].get_text(strip=True)

# 마지막 항목이 설명인지 확인 (가격이나 저자가 아닌 경우)

if "가격:" not in last_info and "저자:" not in last_info:

description = last_info

# 만약 모든 p 태그가 가격/저자면, 다른 p 태그에서 설명 찾기

all_p_tags = row.find_all('p')

if not description and len(all_p_tags) > len(book_info_elems):

for p in all_p_tags:

p_text = p.get_text(strip=True)

if "가격:" not in p_text and "저자:" not in p_text and "book_info" not in str(p.get('class', [])):

description = p_text

break

books.append({

'title': title,

'price': price,

'author': author,

'description': description

})

except Exception as e:

continue

if not books:

return "수집된 책 정보가 없습니다."

# 마크다운 파일 생성

now = datetime.now()

date_str = now.strftime("%Y%m%d")

markdown_content = generate_markdown(books, now)

# c:/reports/ 폴더 생성 (없으면)

reports_dir = Path("c:/reports")

reports_dir.mkdir(parents=True, exist_ok=True)

# 파일 저장

file_name = f"book_report_{date_str}.md"

file_path = reports_dir / file_name

with open(file_path, 'w', encoding='utf-8') as f:

f.write(markdown_content)

return f"✓ 책 정보 수집 완료!\n- 파일 경로: {file_path}\n- 수집된 도서 수: {len(books)}권\n- 수집 날짜: {now.strftime('%Y년 %m월 %d일 %H:%M:%S')}"

except Exception as e:

return f"오류 발생: {str(e)}"

def generate_markdown(books: list, date: datetime) -> str:

"""

수집된 책 정보를 마크다운 형식으로 변환합니다.

Args:

books: 책 정보 딕셔너리 리스트

date: 수집 날짜

Returns:

마크다운 형식의 문자열

"""

content = f"""# 위니브 신간 도서 정보

**수집 날짜:** {date.strftime('%Y년 %m월 %d일')} | **총 도서 수:** {len(books)}권

---

"""

for idx, book in enumerate(books, 1):

content += f"""## {idx}. {book['title']}

"""

if book['author']:

content += f"**저자:** {book['author'].replace('저자: ', '').strip()}\n\n"

if book['price']:

content += f"**가격:** {book['price'].replace('가격: ', '').strip()}\n\n"

if book['description']:

content += f"**소개:** {book['description']}\n\n"

content += "---\n\n"

return content

# 서버 실행하기

if __name__ == "__main__":

mcp.run()
```

### [응용 예제 4] 주가 데이터 자동 수집 및 분석 리포트 생성하기

- 클로드에서 생성한 코드 
```
from mcp.server.fastmcp import FastMCP

import requests

from bs4 import BeautifulSoup

from datetime import datetime

import os

import json

# MCP 서버 생성하기

mcp = FastMCP(name="stock_collector")

def fetch_stock_page():

"""위니브 주식 정보 페이지에서 HTML 가져오기"""

try:

headers = {

'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'

}

response = requests.get('https://paullab.co.kr/stock.html', headers=headers, timeout=10)

response.encoding = 'utf-8'

return response.text

except Exception as e:

raise Exception(f"페이지 수집 실패: {str(e)}")

def parse_stock_data(html_content):

"""HTML에서 주식 데이터 추출"""

soup = BeautifulSoup(html_content, 'html.parser')

companies = {}

# 테이블 찾기

tables = soup.find_all('table')

for table in tables:

rows = table.find_all('tr')

current_company = None

for row in rows:

cells = row.find_all(['td', 'th'])

if not cells:

continue

cell_text = [cell.get_text(strip=True) for cell in cells]

# 회사명 식별

if any(name in str(cell_text) for name in ['제주코딩베이스캠프 연구원', '제주코딩베이스캠프 미디어',

'제주코딩베이스캠프 출판사', '주식회사 위니브']):

for name in ['제주코딩베이스캠프 연구원', '제주코딩베이스캠프 미디어',

'제주코딩베이스캠프 출판사', '주식회사 위니브']:

if name in cell_text[0]:

current_company = name

if current_company not in companies:

companies[current_company] = {

'summary': {},

'daily_data': []

}

break

# 주요 지표 추출 (시가총액, 현재가, 52주 최고/최저)

if current_company and len(cell_text) >= 2:

text = ' '.join(cell_text)

if '시가총액' in text or '현재가' in text or '52주' in text:

if '현재가' in text and len(cell_text) > 1:

try:

price = cell_text[-1].replace(',', '')

companies[current_company]['summary']['current_price'] = price

except:

pass

if '시가총액' in text and len(cell_text) > 1:

companies[current_company]['summary']['market_cap'] = cell_text[-1]

if '52주 최고' in text and len(cell_text) > 1:

companies[current_company]['summary']['52week_high'] = cell_text[-1]

if '52주 최저' in text and len(cell_text) > 1:

companies[current_company]['summary']['52week_low'] = cell_text[-1]

# 일일 주가 데이터 추출 (날짜, 종가, 전일비, 시가, 고가, 저가, 거래량)

if current_company and len(cell_text) >= 7:

try:

# 날짜 형식 확인

if '-' in cell_text[0] or '/' in cell_text[0]:

daily_entry = {

'date': cell_text[0],

'close': cell_text[1].replace(',', '') if len(cell_text) > 1 else '',

'change': cell_text[2] if len(cell_text) > 2 else '',

'open': cell_text[3].replace(',', '') if len(cell_text) > 3 else '',

'high': cell_text[4].replace(',', '') if len(cell_text) > 4 else '',

'low': cell_text[5].replace(',', '') if len(cell_text) > 5 else '',

'volume': cell_text[6].replace(',', '') if len(cell_text) > 6 else ''

}

companies[current_company]['daily_data'].append(daily_entry)

except:

pass

return companies

def generate_markdown_report(companies_data):

"""마크다운 형식의 분석 리포트 생성"""

today = datetime.now()

report = f"""# 위니브 주식 분석 리포트

**작성일자**: {today.strftime('%Y년 %m월 %d일')}

## 1. 회사별 주요 지표 요약

"""

for company_name, data in companies_data.items():

report += f"""### {company_name}

| 지표 | 값 |

|------|-----|

"""

summary = data.get('summary', {})

if summary.get('current_price'):

report += f"| 현재가 | {summary['current_price']} |\n"

if summary.get('market_cap'):

report += f"| 시가총액 | {summary['market_cap']} |\n"

if summary.get('52week_high'):

report += f"| 52주 최고가 | {summary['52week_high']} |\n"

if summary.get('52week_low'):

report += f"| 52주 최저가 | {summary['52week_low']} |\n"

report += "\n"

# 최근 5일간 주가 동향

report += """## 2. 최근 5일간 주가 동향 분석

"""

for company_name, data in companies_data.items():

report += f"""### {company_name}

| 날짜 | 종가 | 변동률 | 시가 | 고가 | 저가 | 거래량 |

|------|------|--------|------|------|------|--------|

"""

daily_data = data.get('daily_data', [])[:5]

for day in daily_data:

report += f"| {day['date']} | {day['close']} | {day['change']} | {day['open']} | {day['high']} | {day['low']} | {day['volume']} |\n"

report += "\n"

# 회사간 비교 분석

report += """## 3. 회사간 비교 분석

"""

report += """| 회사명 | 현재가 | 시가총액 | 52주 최고가 | 52주 최저가 |

|--------|--------|----------|-----------|----------|

"""

for company_name, data in companies_data.items():

summary = data.get('summary', {})

report += f"""| {company_name} | {summary.get('current_price', 'N/A')} | {summary.get('market_cap', 'N/A')} | {summary.get('52week_high', 'N/A')} | {summary.get('52week_low', 'N/A')} |

"""

report += "\n"

# 거래량 및 변동률 순위

report += """## 4. 거래량 및 변동률 순위

"""

volume_data = []

change_data = []

for company_name, data in companies_data.items():

daily_data = data.get('daily_data', [])

if daily_data:

try:

volume = int(daily_data[0]['volume']) if daily_data[0]['volume'] else 0

change = daily_data[0]['change']

volume_data.append((company_name, volume))

change_data.append((company_name, change))

except:

pass

report += "### 거래량 순위 (최근 1일)\n\n"

volume_data.sort(key=lambda x: x[1], reverse=True)

for idx, (company, volume) in enumerate(volume_data, 1):

report += f"{idx}. {company}: {volume:,}\n"

report += "\n### 변동률 순위\n\n"

for idx, (company, change) in enumerate(change_data, 1):

report += f"{idx}. {company}: {change}\n"

report += f"\n---\n*자동 생성된 리포트입니다. ({today.strftime('%Y-%m-%d %H:%M:%S')})*\n"

return report

def ensure_reports_folder():

"""c:/test/reports/ 폴더 생성"""

folder_path = 'c:/test/reports'

os.makedirs(folder_path, exist_ok=True)

return folder_path

@mcp.tool()

def collect_stock_report() -> str:

"""

위니브 주식 정보 페이지에서 데이터를 수집하여 분석 리포트를 생성합니다.

기능:

1. 위니브 주식 정보 페이지(https://paullab.co.kr/stock.html)에서 데이터 수집

2. 회사별 시가총액, 현재가, 52주 최고/최저가 추출

3. 일일 주가 변동 데이터 수집 (날짜, 종가, 전일비, 시가, 고가, 저가, 거래량)

4. 4개 회사 모두 추출

5. 마크다운 형식으로 정리된 리포트 생성

6. c:/test/reports/ 폴더에 stock_report_YYYYMMDD.md 형식으로 저장

반환값: 생성된 리포트의 파일 경로

"""

try:

# 1. 페이지 수집

html_content = fetch_stock_page()

# 2. 데이터 파싱

companies_data = parse_stock_data(html_content)

if not companies_data:

raise Exception("주식 데이터를 추출할 수 없습니다. 페이지 구조가 변경되었을 수 있습니다.")

# 3. 마크다운 리포트 생성

markdown_report = generate_markdown_report(companies_data)

# 4. 폴더 생성 및 파일 저장

folder_path = ensure_reports_folder()

today = datetime.now()

filename = f"stock_report_{today.strftime('%Y%m%d')}.md"

filepath = os.path.join(folder_path, filename)

with open(filepath, 'w', encoding='utf-8') as f:

f.write(markdown_report)

return f"""✅ 주식 분석 리포트가 생성되었습니다.

📄 파일명: {filename}

📍 저장 경로: {filepath}

📊 수집된 회사: {len(companies_data)}개

- {', '.join(companies_data.keys())}

리포트 내용:

- 회사별 주요 지표 요약

- 최근 5일간 주가 동향

- 회사간 비교 분석

- 거래량 및 변동률 순위

마크다운 형식으로 저장되었으며, 분석 및 보고서 작성에 활용할 수 있습니다."""

except Exception as e:

return f"❌ 리포트 생성 실패: {str(e)}"

# 서버 실행하기

if __name__ == "__main__":
mcp.run()
```

## 아래한글(HWP) 파일 읽고 쓰기

[실습]

- server.py 내용
```
from mcp.server.fastmcp import FastMCP

# MCP 서버 생성

mcp = FastMCP(name="server")

@mcp.tool()

def read_hwp(file_name: str) -> str:

"""한글 문서(.hwp)를 읽어 텍스트로 반환합니다.

olefile 라이브러리를 사용하여 한글 문서의 텍스트 내용을 추출합니다.

Args:

file_name (str): 읽을 한글 문서의 이름

예: 'document.hwp'

Returns:

str: 한글 문서에서 추출한 텍스트 내용 또는 오류 메시지

"""

import os

import olefile

# 상대 경로인 경우 현재 디렉토리 기준으로 절대 경로 변환

file_path = os.path.join("c:/test", file_name)

try:

# 한글 파일 열기

if not olefile.isOleFile(file_path):

return f"오류: '{file_path}'는 올바른 한글 문서 형식이 아닙니다."

ole = olefile.OleFileIO(file_path)

# 텍스트 스트림 읽기

if ole.exists("PrvText"):

text_stream = ole.openstream("PrvText")

text_data = text_stream.read().decode("utf-16-le", errors="replace")

ole.close()

return text_data

else:

ole.close()

return "텍스트 내용을 추출할 수 없습니다. 지원되지 않는 한글 문서 형식일 수 있습니다."

except Exception as e:

return f"한글 문서 읽기 오류: {str(e)}"

@mcp.tool()

def write_md_to_hwpx(md_content: str, output_filename: str) -> str:

"""

마크다운 문자열을 .hwpx 파일로 변환합니다.

간단한 구조로, 책 예제용 기본 기능만 제공합니다.

gen.py 파일이 같은 폴더에 함께 있어야 합니다.

"""

import os

import tempfile

import shutil

from gen import create_mimetype_file, create_settings_xml, create_version_xml

from gen import create_preview_text, create_container_files, create_content_hpf

from gen import create_header_xml, create_section_xml

from bs4 import BeautifulSoup

import markdown

import zipfile

try:

# 임시 폴더 생성

temp_dir = tempfile.mkdtemp()

# 마크다운 → HTML → soup

html = markdown.markdown(md_content)

soup = BeautifulSoup(html, "html.parser")

# 제목 추출

h1 = soup.find("h1")

title = h1.text if h1 else "문서"

# 구조 생성

os.makedirs(os.path.join(temp_dir, "META-INF"), exist_ok=True)

os.makedirs(os.path.join(temp_dir, "Contents"), exist_ok=True)

os.makedirs(os.path.join(temp_dir, "Preview"), exist_ok=True)

# 각 구성 요소 생성

create_mimetype_file(temp_dir)

create_settings_xml(temp_dir)

create_version_xml(temp_dir)

create_preview_text(temp_dir, soup)

create_container_files(temp_dir)

create_content_hpf(temp_dir, title)

create_header_xml(temp_dir)

create_section_xml(temp_dir, soup)

# 압축 → HWPX 저장

output_path = os.path.join("c:/test", output_filename)

with zipfile.ZipFile(output_path, "w") as zip_file:

zip_file.write(

os.path.join(temp_dir, "mimetype"),

"mimetype",

compress_type=zipfile.ZIP_STORED,

)

for root, _, files in os.walk(temp_dir):

for file in files:

if file != "mimetype":

file_path = os.path.join(root, file)

arcname = os.path.relpath(file_path, temp_dir)

zip_file.write(

file_path, arcname, compress_type=zipfile.ZIP_DEFLATED

)

shutil.rmtree(temp_dir)

return f"{output_filename} 파일로 변환 완료!"

except Exception as e:

return f"변환 실패: {e}"

# 서버 실행
if __name__ == "__main__":
mcp.run()
```

- gen.py 내용
```
import os

import zipfile

import tempfile

import shutil

import markdown

from bs4 import BeautifulSoup

from datetime import datetime

import uuid

import re

def markdown_to_hwpx(markdown_file, output_file):

"""

마크다운 파일을 HWPX 형식으로 변환합니다.

실제 작동하는 HWPX 파일 구조를 기반으로 만들어졌습니다.

Args:

markdown_file (str): 변환할 마크다운 파일 경로

output_file (str): 출력할 HWPX 파일 경로

"""

# 마크다운 파일 읽기

with open(markdown_file, "r", encoding="utf-8") as f:

markdown_text = f.read()

# 마크다운을 HTML로 변환

html = markdown.markdown(markdown_text)

soup = BeautifulSoup(html, "html.parser")

# 제목 추출 (첫 번째 h1 또는 파일 이름 사용)

title = os.path.splitext(os.path.basename(markdown_file))[0]

h1_tag = soup.find("h1")

if h1_tag:

title = h1_tag.text

# 임시 디렉토리 생성

temp_dir = tempfile.mkdtemp()

try:

# HWPX 파일 구조 생성

os.makedirs(os.path.join(temp_dir, "META-INF"), exist_ok=True)

os.makedirs(os.path.join(temp_dir, "Contents"), exist_ok=True)

os.makedirs(os.path.join(temp_dir, "Preview"), exist_ok=True)

# 기본 파일들 생성

create_mimetype_file(temp_dir)

create_settings_xml(temp_dir)

create_version_xml(temp_dir)

create_preview_text(temp_dir, soup)

create_container_files(temp_dir)

create_content_hpf(temp_dir, title)

create_header_xml(temp_dir)

create_section_xml(temp_dir, soup)

# ZIP 파일로 압축

with zipfile.ZipFile(output_file, "w") as zip_file:

# mimetype 파일은 압축하지 않고 첫 번째로 추가

zip_file.write(

os.path.join(temp_dir, "mimetype"),

"mimetype",

compress_type=zipfile.ZIP_STORED,

)

# 나머지 파일들 추가

for root, _, files in os.walk(temp_dir):

for file in files:

if file != "mimetype": # mimetype은 이미 추가했으므로 건너뜀

file_path = os.path.join(root, file)

arcname = os.path.relpath(file_path, temp_dir)

zip_file.write(

file_path, arcname, compress_type=zipfile.ZIP_DEFLATED

)

print(f"변환 완료: {output_file}")

finally:

# 임시 디렉토리 삭제

shutil.rmtree(temp_dir)

def create_mimetype_file(temp_dir):

"""mimetype 파일 생성"""

with open(os.path.join(temp_dir, "mimetype"), "w", encoding="utf-8") as f:

f.write("application/hwp+zip")

def create_settings_xml(temp_dir):

"""settings.xml 파일 생성"""

settings_content = """<?xml version="1.0" encoding="UTF-8" standalone="yes" ?>

<ha:HWPApplicationSetting xmlns:ha="http://www.hancom.co.kr/hwpml/2011/app" xmlns:config="urn:oasis:names:tc:opendocument:xmlns:config:1.0">

<ha:CaretPosition listIDRef="0" paraIDRef="4" pos="2"/>

</ha:HWPApplicationSetting>"""

with open(os.path.join(temp_dir, "settings.xml"), "w", encoding="utf-8") as f:

f.write(settings_content)

def create_version_xml(temp_dir):

"""version.xml 파일 생성"""

version_content = """<?xml version="1.0" encoding="UTF-8" standalone="yes" ?>

<hv:HCFVersion xmlns:hv="http://www.hancom.co.kr/hwpml/2011/version" tagetApplication="WORDPROCESSOR" major="5" minor="1" micro="0" buildNumber="1" os="1" xmlVersion="1.4" application="Hancom Office Hangul" appVersion="10, 0, 0, 11808 WIN32LEWindows_8"/>"""

with open(os.path.join(temp_dir, "version.xml"), "w", encoding="utf-8") as f:

f.write(version_content)

def create_preview_text(temp_dir, soup):

"""Preview/PrvText.txt 파일 생성"""

preview_text = ""

# 모든 텍스트 요소 추출하여 미리보기 텍스트 생성

for element in soup.find_all(["h1", "h2", "h3", "h4", "h5", "h6", "p"]):

preview_text += element.text + "\n"

# 미리보기 텍스트 제한 (100자 정도)

preview_text = preview_text[:100]

with open(

os.path.join(temp_dir, "Preview", "PrvText.txt"), "w", encoding="utf-8"

) as f:

f.write(preview_text)

def create_container_files(temp_dir):

"""META-INF 디렉토리의 컨테이너 파일들 생성"""

# container.rdf

container_rdf = """<?xml version="1.0" encoding="UTF-8" standalone="yes" ?>

<rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">

<rdf:Description rdf:about="">

<ns0:hasPart xmlns:ns0="http://www.hancom.co.kr/hwpml/2016/meta/pkg#" rdf:resource="Contents/header.xml"/>

</rdf:Description>

<rdf:Description rdf:about="Contents/header.xml">

<rdf:type rdf:resource="http://www.hancom.co.kr/hwpml/2016/meta/pkg#HeaderFile"/>

</rdf:Description>

<rdf:Description rdf:about="">

<ns0:hasPart xmlns:ns0="http://www.hancom.co.kr/hwpml/2016/meta/pkg#" rdf:resource="Contents/section0.xml"/>

</rdf:Description>

<rdf:Description rdf:about="Contents/section0.xml">

<rdf:type rdf:resource="http://www.hancom.co.kr/hwpml/2016/meta/pkg#SectionFile"/>

</rdf:Description>

<rdf:Description rdf:about="">

<rdf:type rdf:resource="http://www.hancom.co.kr/hwpml/2016/meta/pkg#Document"/>

</rdf:Description>

</rdf:RDF>"""

with open(

os.path.join(temp_dir, "META-INF", "container.rdf"), "w", encoding="utf-8"

) as f:

f.write(container_rdf)

# container.xml

container_xml = """<?xml version="1.0" encoding="UTF-8" standalone="yes" ?>

<ocf:container xmlns:ocf="urn:oasis:names:tc:opendocument:xmlns:container" xmlns:hpf="http://www.hancom.co.kr/schema/2011/hpf">

<ocf:rootfiles>

<ocf:rootfile full-path="Contents/content.hpf" media-type="application/hwpml-package+xml"/>

<ocf:rootfile full-path="Preview/PrvText.txt" media-type="text/plain"/>

<ocf:rootfile full-path="META-INF/container.rdf" media-type="application/rdf+xml"/>

</ocf:rootfiles>

</ocf:container>"""

with open(

os.path.join(temp_dir, "META-INF", "container.xml"), "w", encoding="utf-8"

) as f:

f.write(container_xml)

# manifest.xml

manifest_xml = """<?xml version="1.0" encoding="UTF-8" standalone="yes" ?>

<odf:manifest xmlns:odf="urn:oasis:names:tc:opendocument:xmlns:manifest:1.0"/>"""

with open(

os.path.join(temp_dir, "META-INF", "manifest.xml"), "w", encoding="utf-8"

) as f:

f.write(manifest_xml)

def create_content_hpf(temp_dir, title):

"""Contents/content.hpf 파일 생성"""

current_date = datetime.now().strftime("%Y-%m-%dT%H:%M:%SZ")

kr_date = datetime.now().strftime("%Y년 %m월 %d일 %A 오후 %I:%M:%S")

content_hpf = f"""<?xml version="1.0" encoding="UTF-8" standalone="yes" ?>

<opf:package xmlns:ha="http://www.hancom.co.kr/hwpml/2011/app" xmlns:hp="http://www.hancom.co.kr/hwpml/2011/paragraph" xmlns:hp10="http://www.hancom.co.kr/hwpml/2016/paragraph" xmlns:hs="http://www.hancom.co.kr/hwpml/2011/section" xmlns:hc="http://www.hancom.co.kr/hwpml/2011/core" xmlns:hh="http://www.hancom.co.kr/hwpml/2011/head" xmlns:hhs="http://www.hancom.co.kr/hwpml/2011/history" xmlns:hm="http://www.hancom.co.kr/hwpml/2011/master-page" xmlns:hpf="http://www.hancom.co.kr/schema/2011/hpf" xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:opf="http://www.idpf.org/2007/opf/" xmlns:ooxmlchart="http://www.hancom.co.kr/hwpml/2016/ooxmlchart" xmlns:hwpunitchar="http://www.hancom.co.kr/hwpml/2016/HwpUnitChar" xmlns:epub="http://www.idpf.org/2007/ops" xmlns:config="urn:oasis:names:tc:opendocument:xmlns:config:1.0" version="" unique-identifier="" id="">

<opf:metadata>

<opf:title>{title}</opf:title>

<opf:language>ko</opf:language>

<opf:meta name="creator" content="text">Markdown Converter</opf:meta>

<opf:meta name="subject" content="text"/>

<opf:meta name="description" content="text"/>

<opf:meta name="lastsaveby" content="text">Markdown Converter</opf:meta>

<opf:meta name="CreatedDate" content="text">{current_date}</opf:meta>

<opf:meta name="ModifiedDate" content="text">{current_date}</opf:meta>

<opf:meta name="date" content="text">{kr_date}</opf:meta>

<opf:meta name="keyword" content="text"/>

</opf:metadata>

<opf:manifest>

<opf:item id="header" href="Contents/header.xml" media-type="application/xml"/>

<opf:item id="section0" href="Contents/section0.xml" media-type="application/xml"/>

<opf:item id="settings" href="settings.xml" media-type="application/xml"/>

</opf:manifest>

<opf:spine>

<opf:itemref idref="header" linear="yes"/>

<opf:itemref idref="section0" linear="no"/>

</opf:spine>

</opf:package>"""

with open(

os.path.join(temp_dir, "Contents", "content.hpf"), "w", encoding="utf-8"

) as f:

f.write(content_hpf)

def create_header_xml(temp_dir):

"""Contents/header.xml 파일 생성"""

# 여기서는 기본 header.xml 파일을 생성합니다.

# 실제 파일은 매우 길기 때문에 필수 부분만 포함시킵니다.

header_xml = """<?xml version="1.0" encoding="UTF-8" standalone="yes" ?>

<hh:head xmlns:ha="http://www.hancom.co.kr/hwpml/2011/app" xmlns:hp="http://www.hancom.co.kr/hwpml/2011/paragraph" xmlns:hp10="http://www.hancom.co.kr/hwpml/2016/paragraph" xmlns:hs="http://www.hancom.co.kr/hwpml/2011/section" xmlns:hc="http://www.hancom.co.kr/hwpml/2011/core" xmlns:hh="http://www.hancom.co.kr/hwpml/2011/head" xmlns:hhs="http://www.hancom.co.kr/hwpml/2011/history" xmlns:hm="http://www.hancom.co.kr/hwpml/2011/master-page" xmlns:hpf="http://www.hancom.co.kr/schema/2011/hpf" xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:opf="http://www.idpf.org/2007/opf/" xmlns:ooxmlchart="http://www.hancom.co.kr/hwpml/2016/ooxmlchart" xmlns:hwpunitchar="http://www.hancom.co.kr/hwpml/2016/HwpUnitChar" xmlns:epub="http://www.idpf.org/2007/ops" xmlns:config="urn:oasis:names:tc:opendocument:xmlns:config:1.0" version="1.4" secCnt="1">

<hh:beginNum page="1" footnote="1" endnote="1" pic="1" tbl="1" equation="1"/>

<hh:refList>

<hh:fontfaces itemCnt="7">

<hh:fontface lang="HANGUL" fontCnt="2">

<hh:font id="0" face="함초롬돋움" type="TTF" isEmbedded="0">

<hh:typeInfo familyType="FCAT_GOTHIC" weight="6" proportion="4" contrast="0" strokeVariation="1" armStyle="1" letterform="1" midline="1" xHeight="1"/>

</hh:font>

<hh:font id="1" face="함초롬바탕" type="TTF" isEmbedded="0">

<hh:typeInfo familyType="FCAT_GOTHIC" weight="6" proportion="4" contrast="0" strokeVariation="1" armStyle="1" letterform="1" midline="1" xHeight="1"/>

</hh:font>

</hh:fontface>

<hh:fontface lang="LATIN" fontCnt="2">

<hh:font id="0" face="함초롬돋움" type="TTF" isEmbedded="0">

<hh:typeInfo familyType="FCAT_GOTHIC" weight="6" proportion="4" contrast="0" strokeVariation="1" armStyle="1" letterform="1" midline="1" xHeight="1"/>

</hh:font>

<hh:font id="1" face="함초롬바탕" type="TTF" isEmbedded="0">

<hh:typeInfo familyType="FCAT_GOTHIC" weight="6" proportion="4" contrast="0" strokeVariation="1" armStyle="1" letterform="1" midline="1" xHeight="1"/>

</hh:font>

</hh:fontface>

<hh:fontface lang="HANJA" fontCnt="2">

<hh:font id="0" face="함초롬돋움" type="TTF" isEmbedded="0">

<hh:typeInfo familyType="FCAT_GOTHIC" weight="6" proportion="4" contrast="0" strokeVariation="1" armStyle="1" letterform="1" midline="1" xHeight="1"/>

</hh:font>

<hh:font id="1" face="함초롬바탕" type="TTF" isEmbedded="0">

<hh:typeInfo familyType="FCAT_GOTHIC" weight="6" proportion="4" contrast="0" strokeVariation="1" armStyle="1" letterform="1" midline="1" xHeight="1"/>

</hh:font>

</hh:fontface>

<hh:fontface lang="JAPANESE" fontCnt="2">

<hh:font id="0" face="함초롬돋움" type="TTF" isEmbedded="0">

<hh:typeInfo familyType="FCAT_GOTHIC" weight="6" proportion="4" contrast="0" strokeVariation="1" armStyle="1" letterform="1" midline="1" xHeight="1"/>

</hh:font>

<hh:font id="1" face="함초롬바탕" type="TTF" isEmbedded="0">

<hh:typeInfo familyType="FCAT_GOTHIC" weight="6" proportion="4" contrast="0" strokeVariation="1" armStyle="1" letterform="1" midline="1" xHeight="1"/>

</hh:font>

</hh:fontface>

<hh:fontface lang="OTHER" fontCnt="2">

<hh:font id="0" face="함초롬돋움" type="TTF" isEmbedded="0">

<hh:typeInfo familyType="FCAT_GOTHIC" weight="6" proportion="4" contrast="0" strokeVariation="1" armStyle="1" letterform="1" midline="1" xHeight="1"/>

</hh:font>

<hh:font id="1" face="함초롬바탕" type="TTF" isEmbedded="0">

<hh:typeInfo familyType="FCAT_GOTHIC" weight="6" proportion="4" contrast="0" strokeVariation="1" armStyle="1" letterform="1" midline="1" xHeight="1"/>

</hh:font>

</hh:fontface>

<hh:fontface lang="SYMBOL" fontCnt="2">

<hh:font id="0" face="함초롬돋움" type="TTF" isEmbedded="0">

<hh:typeInfo familyType="FCAT_GOTHIC" weight="6" proportion="4" contrast="0" strokeVariation="1" armStyle="1" letterform="1" midline="1" xHeight="1"/>

</hh:font>

<hh:font id="1" face="함초롬바탕" type="TTF" isEmbedded="0">

<hh:typeInfo familyType="FCAT_GOTHIC" weight="6" proportion="4" contrast="0" strokeVariation="1" armStyle="1" letterform="1" midline="1" xHeight="1"/>

</hh:font>

</hh:fontface>

<hh:fontface lang="USER" fontCnt="2">

<hh:font id="0" face="함초롬돋움" type="TTF" isEmbedded="0">

<hh:typeInfo familyType="FCAT_GOTHIC" weight="6" proportion="4" contrast="0" strokeVariation="1" armStyle="1" letterform="1" midline="1" xHeight="1"/>

</hh:font>

<hh:font id="1" face="함초롬바탕" type="TTF" isEmbedded="0">

<hh:typeInfo familyType="FCAT_GOTHIC" weight="6" proportion="4" contrast="0" strokeVariation="1" armStyle="1" letterform="1" midline="1" xHeight="1"/>

</hh:font>

</hh:fontface>

</hh:fontfaces>

<hh:borderFills itemCnt="2">

<hh:borderFill id="1" threeD="0" shadow="0" centerLine="NONE" breakCellSeparateLine="0">

<hh:slash type="NONE" Crooked="0" isCounter="0"/>

<hh:backSlash type="NONE" Crooked="0" isCounter="0"/>

<hh:leftBorder type="NONE" width="0.1 mm" color="#000000"/>

<hh:rightBorder type="NONE" width="0.1 mm" color="#000000"/>

<hh:topBorder type="NONE" width="0.1 mm" color="#000000"/>

<hh:bottomBorder type="NONE" width="0.1 mm" color="#000000"/>

<hh:diagonal type="SOLID" width="0.1 mm" color="#000000"/>

</hh:borderFill>

<hh:borderFill id="2" threeD="0" shadow="0" centerLine="NONE" breakCellSeparateLine="0">

<hh:slash type="NONE" Crooked="0" isCounter="0"/>

<hh:backSlash type="NONE" Crooked="0" isCounter="0"/>

<hh:leftBorder type="NONE" width="0.1 mm" color="#000000"/>

<hh:rightBorder type="NONE" width="0.1 mm" color="#000000"/>

<hh:topBorder type="NONE" width="0.1 mm" color="#000000"/>

<hh:bottomBorder type="NONE" width="0.1 mm" color="#000000"/>

<hh:diagonal type="SOLID" width="0.1 mm" color="#000000"/>

<hc:fillBrush>

<hc:winBrush faceColor="none" hatchColor="#999999" alpha="0"/>

</hc:fillBrush>

</hh:borderFill>

</hh:borderFills>

<hh:charProperties itemCnt="11">

<hh:charPr id="0" height="1000" textColor="#000000" shadeColor="none" useFontSpace="0" useKerning="0" symMark="NONE" borderFillIDRef="2">

<hh:fontRef hangul="0" latin="0" hanja="0" japanese="0" other="0" symbol="0" user="0"/>

<hh:ratio hangul="100" latin="100" hanja="100" japanese="100" other="100" symbol="100" user="100"/>

<hh:spacing hangul="0" latin="0" hanja="0" japanese="0" other="0" symbol="0" user="0"/>

<hh:relSz hangul="100" latin="100" hanja="100" japanese="100" other="100" symbol="100" user="100"/>

<hh:offset hangul="0" latin="0" hanja="0" japanese="0" other="0" symbol="0" user="0"/>

<hh:underline type="NONE" shape="SOLID" color="#000000"/>

<hh:strikeout shape="NONE" color="#000000"/>

<hh:outline type="NONE"/>

<hh:shadow type="NONE" color="#B2B2B2" offsetX="10" offsetY="10"/>

</hh:charPr>

<hh:charPr id="6" height="1000" textColor="#000000" shadeColor="none" useFontSpace="0" useKerning="0" symMark="NONE" borderFillIDRef="2">

<hh:fontRef hangul="1" latin="1" hanja="1" japanese="1" other="1" symbol="1" user="1"/>

<hh:ratio hangul="100" latin="100" hanja="100" japanese="100" other="100" symbol="100" user="100"/>

<hh:spacing hangul="0" latin="0" hanja="0" japanese="0" other="0" symbol="0" user="0"/>

<hh:relSz hangul="100" latin="100" hanja="100" japanese="100" other="100" symbol="100" user="100"/>

<hh:offset hangul="0" latin="0" hanja="0" japanese="0" other="0" symbol="0" user="0"/>

<hh:underline type="NONE" shape="SOLID" color="#000000"/>

<hh:strikeout shape="NONE" color="#000000"/>

<hh:outline type="NONE"/>

<hh:shadow type="NONE" color="#B2B2B2" offsetX="10" offsetY="10"/>

</hh:charPr>

<hh:charPr id="7" height="1000" textColor="#000000" shadeColor="none" useFontSpace="0" useKerning="0" symMark="NONE" borderFillIDRef="2">

<hh:fontRef hangul="1" latin="1" hanja="1" japanese="1" other="1" symbol="1" user="1"/>

<hh:ratio hangul="100" latin="100" hanja="100" japanese="100" other="100" symbol="100" user="100"/>

<hh:spacing hangul="0" latin="0" hanja="0" japanese="0" other="0" symbol="0" user="0"/>

<hh:relSz hangul="100" latin="100" hanja="100" japanese="100" other="100" symbol="100" user="100"/>

<hh:offset hangul="0" latin="0" hanja="0" japanese="0" other="0" symbol="0" user="0"/>

<hh:bold/>

<hh:underline type="NONE" shape="SOLID" color="#000000"/>

<hh:strikeout shape="NONE" color="#000000"/>

<hh:outline type="NONE"/>

<hh:shadow type="NONE" color="#B2B2B2" offsetX="10" offsetY="10"/>

</hh:charPr>

<hh:charPr id="8" height="1000" textColor="#000000" shadeColor="none" useFontSpace="0" useKerning="0" symMark="NONE" borderFillIDRef="2">

<hh:fontRef hangul="1" latin="1" hanja="1" japanese="1" other="1" symbol="1" user="1"/>

<hh:ratio hangul="100" latin="100" hanja="100" japanese="100" other="100" symbol="100" user="100"/>

<hh:spacing hangul="0" latin="0" hanja="0" japanese="0" other="0" symbol="0" user="0"/>

<hh:relSz hangul="100" latin="100" hanja="100" japanese="100" other="100" symbol="100" user="100"/>

<hh:offset hangul="0" latin="0" hanja="0" japanese="0" other="0" symbol="0" user="0"/>

<hh:italic/>

<hh:underline type="NONE" shape="SOLID" color="#000000"/>

<hh:strikeout shape="NONE" color="#000000"/>

<hh:outline type="NONE"/>

<hh:shadow type="NONE" color="#B2B2B2" offsetX="10" offsetY="10"/>

</hh:charPr>

<hh:charPr id="9" height="1200" textColor="#000000" shadeColor="none" useFontSpace="0" useKerning="0" symMark="NONE" borderFillIDRef="2">

<hh:fontRef hangul="1" latin="1" hanja="1" japanese="1" other="1" symbol="1" user="1"/>

<hh:ratio hangul="100" latin="100" hanja="100" japanese="100" other="100" symbol="100" user="100"/>

<hh:spacing hangul="0" latin="0" hanja="0" japanese="0" other="0" symbol="0" user="0"/>

<hh:relSz hangul="100" latin="100" hanja="100" japanese="100" other="100" symbol="100" user="100"/>

<hh:offset hangul="0" latin="0" hanja="0" japanese="0" other="0" symbol="0" user="0"/>

<hh:underline type="NONE" shape="SOLID" color="#000000"/>

<hh:strikeout shape="NONE" color="#000000"/>

<hh:outline type="NONE"/>

<hh:shadow type="NONE" color="#B2B2B2" offsetX="10" offsetY="10"/>

</hh:charPr>

<hh:charPr id="10" height="1400" textColor="#000000" shadeColor="none" useFontSpace="0" useKerning="0" symMark="NONE" borderFillIDRef="2">

<hh:fontRef hangul="1" latin="1" hanja="1" japanese="1" other="1" symbol="1" user="1"/>

<hh:ratio hangul="100" latin="100" hanja="100" japanese="100" other="100" symbol="100" user="100"/>

<hh:spacing hangul="0" latin="0" hanja="0" japanese="0" other="0" symbol="0" user="0"/>

<hh:relSz hangul="100" latin="100" hanja="100" japanese="100" other="100" symbol="100" user="100"/>

<hh:offset hangul="0" latin="0" hanja="0" japanese="0" other="0" symbol="0" user="0"/>

<hh:underline type="NONE" shape="SOLID" color="#000000"/>

<hh:strikeout shape="NONE" color="#000000"/>

<hh:outline type="NONE"/>

<hh:shadow type="NONE" color="#B2B2B2" offsetX="10" offsetY="10"/>

</hh:charPr>

</hh:charProperties>

<hh:paraProperties itemCnt="19">

<hh:paraPr id="0" tabPrIDRef="0" condense="0" fontLineHeight="0" snapToGrid="1" suppressLineNumbers="0" checked="0">

<hh:align horizontal="JUSTIFY" vertical="BASELINE"/>

<hh:heading type="NONE" idRef="0" level="0"/>

<hh:breakSetting breakLatinWord="KEEP_WORD" breakNonLatinWord="KEEP_WORD" widowOrphan="0" keepWithNext="0" keepLines="0" pageBreakBefore="0" lineWrap="BREAK"/>

<hh:autoSpacing eAsianEng="0" eAsianNum="0"/>

<hp:switch>

<hp:case hp:required-namespace="http://www.hancom.co.kr/hwpml/2016/HwpUnitChar">

<hh:margin>

<hc:intent value="0" unit="HWPUNIT"/>

<hc:left value="0" unit="HWPUNIT"/>

<hc:right value="0" unit="HWPUNIT"/>

<hc:prev value="0" unit="HWPUNIT"/>

<hc:next value="0" unit="HWPUNIT"/>

</hh:margin>

<hh:lineSpacing type="PERCENT" value="160" unit="HWPUNIT"/>

</hp:case>

<hp:default>

<hh:margin>

<hc:intent value="0" unit="HWPUNIT"/>

<hc:left value="0" unit="HWPUNIT"/>

<hc:right value="0" unit="HWPUNIT"/>

<hc:prev value="0" unit="HWPUNIT"/>

<hc:next value="0" unit="HWPUNIT"/>

</hh:margin>

<hh:lineSpacing type="PERCENT" value="160" unit="HWPUNIT"/>

</hp:default>

</hp:switch>

<hh:border borderFillIDRef="2" offsetLeft="0" offsetRight="0" offsetTop="0" offsetBottom="0" connect="0" ignoreMargin="0"/>

</hh:paraPr>

</hh:paraProperties>

<hh:styles itemCnt="21">

<hh:style id="0" type="PARA" name="바탕글" engName="Normal" paraPrIDRef="0" charPrIDRef="6" nextStyleIDRef="0" langID="1042" lockForm="0"/>

</hh:styles>

</hh:refList>

<hh:compatibleDocument targetProgram="HWP201X">

<hh:layoutCompatibility/>

</hh:compatibleDocument>

<hh:docOption>

<hh:linkinfo path="" pageInherit="0" footnoteInherit="0"/>

</hh:docOption>

<hh:trackchageConfig flags="56"/>

</hh:head>"""

with open(

os.path.join(temp_dir, "Contents", "header.xml"), "w", encoding="utf-8"

) as f:

f.write(header_xml)

def create_section_xml(temp_dir, soup):

"""마크다운 내용을 바탕으로 section0.xml 파일 생성"""

# section 시작 부분

section_start = """<?xml version="1.0" encoding="UTF-8" standalone="yes" ?>

<hs:sec xmlns:ha="http://www.hancom.co.kr/hwpml/2011/app" xmlns:hp="http://www.hancom.co.kr/hwpml/2011/paragraph" xmlns:hp10="http://www.hancom.co.kr/hwpml/2016/paragraph" xmlns:hs="http://www.hancom.co.kr/hwpml/2011/section" xmlns:hc="http://www.hancom.co.kr/hwpml/2011/core" xmlns:hh="http://www.hancom.co.kr/hwpml/2011/head" xmlns:hhs="http://www.hancom.co.kr/hwpml/2011/history" xmlns:hm="http://www.hancom.co.kr/hwpml/2011/master-page" xmlns:hpf="http://www.hancom.co.kr/schema/2011/hpf" xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:opf="http://www.idpf.org/2007/opf/" xmlns:ooxmlchart="http://www.hancom.co.kr/hwpml/2016/ooxmlchart" xmlns:hwpunitchar="http://www.hancom.co.kr/hwpml/2016/HwpUnitChar" xmlns:epub="http://www.idpf.org/2007/ops" xmlns:config="urn:oasis:names:tc:opendocument:xmlns:config:1.0">"""

# 첫 번째 단락에는 섹션 속성 포함

first_p = """<hp:p id="2692885478" paraPrIDRef="0" styleIDRef="0" pageBreak="0" columnBreak="0" merged="0">

<hp:run charPrIDRef="6">

<hp:secPr id="" textDirection="HORIZONTAL" spaceColumns="1134" tabStop="8000" tabStopVal="4000" tabStopUnit="HWPUNIT" outlineShapeIDRef="1" memoShapeIDRef="0" textVerticalWidthHead="0" masterPageCnt="0">

<hp:grid lineGrid="0" charGrid="0" wonggojiFormat="0"/>

<hp:startNum pageStartsOn="BOTH" page="0" pic="0" tbl="0" equation="0"/>

<hp:visibility hideFirstHeader="0" hideFirstFooter="0" hideFirstMasterPage="0" border="SHOW_ALL" fill="SHOW_ALL" hideFirstPageNum="0" hideFirstEmptyLine="0" showLineNumber="0"/>

<hp:lineNumberShape restartType="0" countBy="0" distance="0" startNumber="0"/>

<hp:pagePr landscape="WIDELY" width="59528" height="84186" gutterType="LEFT_ONLY">

<hp:margin header="4252" footer="4252" gutter="0" left="8504" right="8504" top="5668" bottom="4252"/>

</hp:pagePr>

<hp:footNotePr>

<hp:autoNumFormat type="DIGIT" userChar="" prefixChar="" suffixChar=")" supscript="0"/>

<hp:noteLine length="-1" type="SOLID" width="0.12 mm" color="#000000"/>

<hp:noteSpacing betweenNotes="283" belowLine="567" aboveLine="850"/>

<hp:numbering type="CONTINUOUS" newNum="1"/>

<hp:placement place="EACH_COLUMN" beneathText="0"/>

</hp:footNotePr>

<hp:endNotePr>

<hp:autoNumFormat type="DIGIT" userChar="" prefixChar="" suffixChar=")" supscript="0"/>

<hp:noteLine length="14692344" type="SOLID" width="0.12 mm" color="#000000"/>

<hp:noteSpacing betweenNotes="0" belowLine="567" aboveLine="850"/>

<hp:numbering type="CONTINUOUS" newNum="1"/>

<hp:placement place="END_OF_DOCUMENT" beneathText="0"/>

</hp:endNotePr>

<hp:pageBorderFill type="BOTH" borderFillIDRef="1" textBorder="PAPER" headerInside="0" footerInside="0" fillArea="PAPER">

<hp:offset left="1417" right="1417" top="1417" bottom="1417"/>

</hp:pageBorderFill>

<hp:pageBorderFill type="EVEN" borderFillIDRef="1" textBorder="PAPER" headerInside="0" footerInside="0" fillArea="PAPER">

<hp:offset left="1417" right="1417" top="1417" bottom="1417"/>

</hp:pageBorderFill>

<hp:pageBorderFill type="ODD" borderFillIDRef="1" textBorder="PAPER" headerInside="0" footerInside="0" fillArea="PAPER">

<hp:offset left="1417" right="1417" top="1417" bottom="1417"/>

</hp:pageBorderFill>

</hp:secPr>

<hp:ctrl>

<hp:colPr id="" type="NEWSPAPER" layout="LEFT" colCount="1" sameSz="1" sameGap="0"/>

</hp:ctrl>

</hp:run>"""

# 마크다운 요소 처리

paragraphs = []

vertical_pos = 0

for element in soup.find_all(["h1", "h2", "h3", "h4", "h5", "h6", "p", "ul", "ol"]):

if element.name.startswith("h"):

# 제목 처리

level = int(element.name[1])

char_style = "7" # 굵게

font_size = (

1000 + (level == 1) * 400 + (level == 2) * 200

) # h1: 1400, h2: 1200, 나머지: 1000

paragraph = f"""

<hp:run charPrIDRef="{char_style}">

<hp:t>{element.text}</hp:t>

</hp:run>

<hp:linesegarray>

<hp:lineseg textpos="0" vertpos="{vertical_pos}" vertsize="{font_size}" textheight="{font_size}" baseline="{int(font_size*0.85)}" spacing="{int(font_size*0.6)}" horzpos="0" horzsize="42520" flags="393216"/>

</hp:linesegarray>"""

elif element.name == "p":

# 단락 처리

paragraph = f"""

<hp:run charPrIDRef="6">

<hp:t>{element.text}</hp:t>

</hp:run>

<hp:linesegarray>

<hp:lineseg textpos="0" vertpos="{vertical_pos}" vertsize="1000" textheight="1000" baseline="850" spacing="600" horzpos="0" horzsize="42520" flags="393216"/>

</hp:linesegarray>"""

elif element.name in ["ul", "ol"]:

# 목록 처리

paragraph = ""

for li in element.find_all("li"):

item_text = "• " + li.text

paragraph += f"""

<hp:run charPrIDRef="6">

<hp:t>{item_text}</hp:t>

</hp:run>

<hp:linesegarray>

<hp:lineseg textpos="0" vertpos="{vertical_pos}" vertsize="1000" textheight="1000" baseline="850" spacing="600" horzpos="0" horzsize="42520" flags="393216"/>

</hp:linesegarray>"""

vertical_pos += 1600

# 다음 요소의 수직 위치 업데이트

vertical_pos += 1600

# 첫 번째 단락은 특별 처리

if len(paragraphs) == 0:

paragraphs.append(first_p + paragraph + "</hp:p>")

else:

paragraphs.append(

f"""<hp:p id="0" paraPrIDRef="0" styleIDRef="0" pageBreak="0" columnBreak="0" merged="0">{paragraph}</hp:p>"""

)

# 내용이 없는 경우, 빈 단락 추가

if not paragraphs:

paragraphs.append(

first_p

+ """

<hp:run charPrIDRef="6">

<hp:t>샘플 내용</hp:t>

</hp:run>

<hp:linesegarray>

<hp:lineseg textpos="0" vertpos="0" vertsize="1000" textheight="1000" baseline="850" spacing="600" horzpos="0" horzsize="42520" flags="393216"/>

</hp:linesegarray></hp:p>"""

)

# 섹션 종료

section_end = """</hs:sec>"""

# 최종 XML 조합

section_xml = section_start + "".join(paragraphs) + section_end

with open(

os.path.join(temp_dir, "Contents", "section0.xml"), "w", encoding="utf-8"

) as f:

f.write(section_xml)

if __name__ == "__main__":

# 실행 예시

markdown_to_hwpx("sample.md", "output.hwpx")
```

- sample.md 내용
```
# 제목 1

## 제목 2

일반 텍스트 단락입니다.

- 리스트 아이템 1

- 리스트 아이템 2
```

## 공공기관 문서 검토 업무 자동화

- 클로드가 제공한 프롬프트를 토대로 자동 생성한 코드 예

```
import os

import re

from datetime import datetime

from pathlib import Path

import olefile

from mcp.server.fastmcp import FastMCP

mcp = FastMCP(name="hwp_document_reviewer")

class DocumentInfo:

"""문서 정보 구조"""

def __init__(self, filename: str):

self.filename = filename

self.title = ""

self.sections = []

self.budget_info = []

self.schedule_info = []

self.contact_info = []

self.special_notes = []

def read_hwp_text(file_path: str) -> str:

"""

아래아한글 파일(.hwp)을 읽어 텍스트를 추출합니다.

Parameters

----------

file_path : str

읽을 .hwp 파일의 전체 경로

Returns

-------

str

추출된 텍스트 내용

"""

try:

if not olefile.isOleFile(file_path):

return f"오류: {file_path}는 유효한 HWP 파일이 아닙니다."

ole = olefile.OleFileIO(file_path)

# PrvText 스트림에서 텍스트 추출

if ole.exists('PrvText'):

encoded_text = ole.openstream('PrvText').read()

# utf-16-le 인코딩으로 디코딩

decoded_text = encoded_text.decode('utf-16-le', errors='ignore')

ole.close()

return decoded_text

else:

ole.close()

return "오류: PrvText 스트림을 찾을 수 없습니다."

except Exception as e:

return f"오류 발생: {str(e)}"

def extract_document_info(text: str, filename: str) -> DocumentInfo:

"""

텍스트에서 주요 정보를 추출합니다.

Parameters

----------

text : str

문서 텍스트

filename : str

파일명

Returns

-------

DocumentInfo

추출된 문서 정보

"""

doc_info = DocumentInfo(filename=filename)

lines = text.split('\n')

# 제목 추출 (첫 번째 비어있지 않은 줄)

for line in lines:

clean_line = line.strip()

if clean_line and len(clean_line) > 2:

doc_info.title = clean_line

break

# 섹션, 예산, 일정, 담당자, 특이사항 추출

budget_keywords = ['예산', '비용', '금액', '원', '만원', '억원']

schedule_keywords = ['일정', '기간', '날짜', '월', '년', '마감', '완료일']

contact_keywords = ['담당자', '책임자', '연락처', '전화', '이메일', '부서']

special_keywords = ['특이사항', '주의', '참고', '비고', '중요']

section_pattern = re.compile(r'^[0-9\.\s]*[가-힣]{2,}.*[:：]?\s*$')

for i, line in enumerate(lines):

clean_line = line.strip()

if not clean_line:

continue

# 섹션 헤더 감지

if section_pattern.match(clean_line) and len(clean_line) < 50:

doc_info.sections.append(clean_line)

# 예산 정보 추출

if any(keyword in clean_line for keyword in budget_keywords):

if clean_line not in doc_info.budget_info:

doc_info.budget_info.append(clean_line)

# 일정 정보 추출

if any(keyword in clean_line for keyword in schedule_keywords):

if clean_line not in doc_info.schedule_info:

doc_info.schedule_info.append(clean_line)

# 담당자 정보 추출

if any(keyword in clean_line for keyword in contact_keywords):

if clean_line not in doc_info.contact_info:

doc_info.contact_info.append(clean_line)

# 특이사항 추출

if any(keyword in clean_line for keyword in special_keywords):

if clean_line not in doc_info.special_notes:

doc_info.special_notes.append(clean_line)

# 다음 2-3줄도 특이사항에 포함

for j in range(1, 4):

if i + j < len(lines):

next_line = lines[i + j].strip()

if next_line and next_line not in doc_info.special_notes:

doc_info.special_notes.append(next_line)

return doc_info

def generate_review_points(doc_info: DocumentInfo) -> list[str]:

"""

문서 정보를 바탕으로 주요 검토 포인트를 생성합니다.

Parameters

----------

doc_info : DocumentInfo

문서 정보

Returns

-------

list[str]

검토 포인트 목록

"""

points = []

if doc_info.budget_info:

points.append("예산 관련: 금액의 적정성 및 산출 근거 확인 필요")

else:

points.append("예산 관련: 예산 정보가 명시되지 않음 - 확인 필요")

if doc_info.schedule_info:

points.append("일정 관련: 제시된 일정의 실현 가능성 검토 필요")

else:

points.append("일정 관련: 구체적인 일정이 명시되지 않음 - 추가 필요")

if doc_info.contact_info:

points.append("담당자 관련: 책임 소재 및 연락처 확인됨")

else:

points.append("담당자 관련: 담당자 정보 누락 - 명시 필요")

if doc_info.special_notes:

points.append("특이사항: 명시된 사항에 대한 세부 검토 필요")

if not doc_info.sections or len(doc_info.sections) < 3:

points.append("문서 구조: 섹션 구성이 부족함 - 체계적 정리 권장")

return points

def generate_analysis_opinion(doc_info: DocumentInfo) -> list[str]:

"""

문서에 대한 분석 의견을 생성합니다.

Parameters

----------

doc_info : DocumentInfo

문서 정보

Returns

-------

list[str]

분석 의견 목록

"""

opinions = []

# 완성도 평가

completeness_score = 0

if doc_info.budget_info: completeness_score += 1

if doc_info.schedule_info: completeness_score += 1

if doc_info.contact_info: completeness_score += 1

if len(doc_info.sections) >= 3: completeness_score += 1

if completeness_score >= 3:

opinions.append("문서 완성도: 양호 - 주요 정보가 대부분 포함되어 있음")

elif completeness_score >= 2:

opinions.append("문서 완성도: 보통 - 일부 정보 보완 필요")

else:

opinions.append("문서 완성도: 미흡 - 필수 정보 대폭 보완 필요")

# 구체성 평가

if doc_info.budget_info and doc_info.schedule_info:

opinions.append("정보의 구체성: 예산 및 일정이 명시되어 실행 가능성 검토 가능")

else:

opinions.append("정보의 구체성: 예산 또는 일정 정보 부족으로 실행 계획 구체화 필요")

# 책임 소재

if doc_info.contact_info:

opinions.append("책임 체계: 담당자가 명확히 지정되어 있음")

else:

opinions.append("책임 체계: 담당자 지정 필요")

return opinions

def generate_markdown_report(documents_info: list[DocumentInfo]) -> str:

"""

문서 정보들을 마크다운 리포트로 변환합니다.

Parameters

----------

documents_info : list[DocumentInfo]

문서 정보 목록

Returns

-------

str

마크다운 형식의 리포트

"""

report = f"# 문서 검토 리포트\n\n"

report += f"**검토 일시**: {datetime.now().strftime('%Y년 %m월 %d일 %H:%M')}\n\n"

report += f"**검토 문서 수**: {len(documents_info)}건\n\n"

report += "---\n\n"

# 전체 요약

report += "## 📊 전체 요약\n\n"

total_budget = sum(len(doc.budget_info) for doc in documents_info)

total_schedule = sum(len(doc.schedule_info) for doc in documents_info)

total_contact = sum(len(doc.contact_info) for doc in documents_info)

report += f"- 예산 정보 포함 문서: {total_budget}건\n"

report += f"- 일정 정보 포함 문서: {total_schedule}건\n"

report += f"- 담당자 정보 포함 문서: {total_contact}건\n\n"

report += "---\n\n"

# 개별 문서 분석

for idx, doc_info in enumerate(documents_info, 1):

report += f"## 📄 문서 {idx}: {doc_info.filename}\n\n"

# 제목

report += f"### 문서 제목\n\n"

report += f"{doc_info.title if doc_info.title else '(제목 없음)'}\n\n"

# 주요 섹션

report += f"### 주요 섹션\n\n"

if doc_info.sections:

for section in doc_info.sections[:5]: # 상위 5개만

report += f"- {section}\n"

else:

report += "- (섹션 정보 없음)\n"

report += "\n"

# 예산 정보

report += f"### 💰 예산 정보\n\n"

if doc_info.budget_info:

for budget in doc_info.budget_info:

report += f"- {budget}\n"

else:

report += "- (예산 정보 없음)\n"

report += "\n"

# 일정 정보

report += f"### 📅 일정 정보\n\n"

if doc_info.schedule_info:

for schedule in doc_info.schedule_info:

report += f"- {schedule}\n"

else:

report += "- (일정 정보 없음)\n"

report += "\n"

# 담당자 정보

report += f"### 👤 담당자 정보\n\n"

if doc_info.contact_info:

for contact in doc_info.contact_info:

report += f"- {contact}\n"

else:

report += "- (담당자 정보 없음)\n"

report += "\n"

# 특이사항

report += f"### ⚠ 특이사항\n\n"

if doc_info.special_notes:

for note in doc_info.special_notes[:3]: # 상위 3개만

report += f"- {note}\n"

else:

report += "- (특이사항 없음)\n"

report += "\n"

# 검토 포인트

report += f"### 🔍 주요 검토 포인트\n\n"

review_points = generate_review_points(doc_info)

for point in review_points:

report += f"- {point}\n"

report += "\n"

# 분석 의견

report += f"### 💡 분석 의견\n\n"

opinions = generate_analysis_opinion(doc_info)

for opinion in opinions:

report += f"- {opinion}\n"

report += "\n"

report += "---\n\n"

# 종합 의견

report += "## 📝 종합 의견\n\n"

report += "### 권고사항\n\n"

incomplete_docs = [doc for doc in documents_info

if not doc.budget_info or not doc.schedule_info or not doc.contact_info]

if incomplete_docs:

report += f"1. **정보 보완 필요 문서**: {len(incomplete_docs)}건\n"

for doc in incomplete_docs:

report += f" - {doc.filename}\n"

report += "\n"

report += "2. **공통 개선사항**\n"

report += " - 문서 양식의 표준화 검토\n"

report += " - 필수 기재 사항 체크리스트 적용\n"

report += " - 담당자 및 검토자 서명란 추가\n\n"

report += "### 우선순위\n\n"

report += "1. 🔴 **긴급**: 예산·일정·담당자 모두 누락된 문서 보완\n"

report += "2. 🟡 **중요**: 예산 또는 일정 정보 누락 문서 보완\n"

report += "3. 🟢 **일반**: 특이사항 및 세부 내용 검토\n\n"

report += f"**리포트 생성 완료**: {datetime.now().strftime('%Y-%m-%d %H:%M:%S')}\n"

return report

@mcp.tool()

def review_hwp_documents() -> str:

"""

c:/test/project 폴더의 아래아한글 문서들을 일괄 분석하여 검토 리포트를 생성합니다.

각 문서에서 다음 정보를 자동으로 추출합니다:

- 제목 및 주요 섹션

- 예산 관련 정보

- 일정 관련 정보

- 담당자 정보

- 특이사항

추출된 정보를 바탕으로 주요 검토 포인트와 분석 의견을 제시하며,

'review_report_YYYYMMDD.md' 형식으로 c:/test/reports/ 폴더에 저장합니다.

Returns

-------

str

분석 결과 요약 메시지

"""

try:

# 폴더 경로 설정

project_folder = Path("c:/test/project")

reports_folder = Path("c:/test/reports")

# 리포트 폴더 생성

reports_folder.mkdir(parents=True, exist_ok=True)

# .hwp 파일 찾기

hwp_files = list(project_folder.glob("*.hwp"))

if not hwp_files:

return "❌ c:/test/project 폴더에 .hwp 파일이 없습니다."

# 각 문서 분석

documents_info = []

for hwp_file in hwp_files:

text = read_hwp_text(str(hwp_file))

if not text.startswith("오류"):

doc_info = extract_document_info(text, hwp_file.name)

documents_info.append(doc_info)

if not documents_info:

return "❌ 문서를 읽을 수 없거나 분석할 수 있는 문서가 없습니다."

# 마크다운 리포트 생성

report_content = generate_markdown_report(documents_info)

# 리포트 파일명 생성

today = datetime.now().strftime("%Y%m%d")

report_filename = f"review_report_{today}.md"

report_path = reports_folder / report_filename

# 리포트 저장

with open(report_path, 'w', encoding='utf-8') as f:

f.write(report_content)

# 결과 메시지

result_message = f"""

📊 문서 검토 분석 완료!

✅ 분석된 문서: {len(documents_info)}건

📁 리포트 저장 위치: {report_path}

📋 분석 요약:

- 예산 정보 포함: {sum(1 for doc in documents_info if doc.budget_info)}건

- 일정 정보 포함: {sum(1 for doc in documents_info if doc.schedule_info)}건

- 담당자 정보 포함: {sum(1 for doc in documents_info if doc.contact_info)}건

- 특이사항 포함: {sum(1 for doc in documents_info if doc.special_notes)}건

💡 상세한 검토 내용은 생성된 리포트 파일을 확인하세요.

"""

return result_message

except Exception as e:

return f"❌ 오류 발생: {str(e)}"

if __name__ == "__main__":

mcp.run()
```

## P.83 3) 엑셀 파일 읽고 쓰기

- [실습1 ] excel_pandas.py 코드 작성

```
import pandas as pd

# 간단한 데이터 생성

data = [

    {"name": "김철수", "age": 30, "department": "개발팀"},

    {"name": "이영희", "age": 35, "department": "마케팅팀"},

    {"name": "박지민", "age": 28, "department": "인사팀"},

]

# 데이터프레임 생성

df = pd.DataFrame(data)

# 엑셀 파일로 저장

df.to_excel("c:/test/employees.xlsx", index=False)

# 엑셀 파일 읽기

read_df = pd.read_excel("c:/test/employees.xlsx")

print(read_df)
```

- python excel_pandas.py 실행후 c:\test 폴더에 .xlsx 파일이 있는지 열어서 확인

- [실습2] excel_xlsxriter_1.py 작성
```
import xlsxwriter

# 엑셀 파일 생성하기(test.xlsx로 생성)

workbook = xlsxwriter.Workbook("c:/test/test.xlsx")

# 파일 안에 워크 시트 생성하기(test이름으로 생성, 여러개의 워크시트 만들 수 있음)

worksheet = workbook.add_worksheet("test")

# 워크 시트 안에 문자열 값을 넣습니다.

worksheet.write("A1", "A")

worksheet.write("B1", "B")

worksheet.write("C1", "C")

worksheet.write("D1", "D")

worksheet.write("E1", "E")

# 워크 시트 안에 숫자 값을 넣습니다.

worksheet.write("A2", 1)

worksheet.write("B2", 2)

worksheet.write("C2", 3)

worksheet.write("D2", 4)

worksheet.write("E2", 5)

# 워크 시트 안에 숫자 값을 넣습니다.

worksheet.write(2, 0, 1)

worksheet.write(2, 1, 2)

worksheet.write(2, 2, 3)

worksheet.write(2, 3, 4)

worksheet.write(2, 4, 5)

workbook.close()
```

- [실습 3] excel_xslxwriter_2.py 코드 작성

```
import xlsxwriter

# 엑셀 파일 생성

workbook = xlsxwriter.Workbook("c:/test/employee_details.xlsx")

# 워크시트 추가

worksheet = workbook.add_worksheet("직원정보")

summary_sheet = workbook.add_worksheet("요약")

# 셀 서식 정의

header_format = workbook.add_format(

    {

        "bold": True,

        "font_color": "white",

        "bg_color": "#4472C4",

        "align": "center",

        "valign": "vcenter",

        "border": 1,

    }

)

date_format = workbook.add_format({"num_format": "yyyy-mm-dd"})

money_format = workbook.add_format({"num_format": "#,##0"})

percent_format = workbook.add_format({"num_format": "0.0%"})

border_format = workbook.add_format({"border": 1})

# 열 너비 설정

worksheet.set_column("A:A", 15)

worksheet.set_column("B:B", 10)

worksheet.set_column("C:C", 15)

worksheet.set_column("D:D", 15)

worksheet.set_column("E:E", 15)

# 헤더 추가

headers = ["이름", "나이", "부서", "입사일", "연봉"]

for col, header in enumerate(headers):

worksheet.write(0, col, header, header_format)

# 데이터 추가

employee_data = [

    ["김철수", 30, "개발팀", "2021-01-15", 45000000],

    ["이영희", 35, "마케팅팀", "2019-05-20", 55000000],

    ["박지민", 28, "인사팀", "2022-03-10", 42000000],

    ["최유진", 32, "영업팀", "2020-11-05", 60000000],

    ["정민수", 27, "개발팀", "2022-08-22", 43000000],

]

# 데이터 행 채우기

for row_num, employee in enumerate(employee_data):

    worksheet.write(row_num + 1, 0, employee[0], border_format) # 이름

    worksheet.write(row_num + 1, 1, employee[1], border_format) # 나이

    worksheet.write(row_num + 1, 2, employee[2], border_format) # 부서

    worksheet.write(row_num + 1, 3, employee[3], date_format) # 입사일

    worksheet.write(row_num + 1, 4, employee[4], money_format) # 연봉

# 조건부 서식 추가 (연봉 5000만원 이상인 경우 배경색 변경)

worksheet.conditional_format(

    "E2:E6",

    {

        "type": "cell",

        "criteria": ">=",

        "value": 50000000,

        "format": workbook.add_format({"bg_color": "#C6EFCE"}),

    },

)

# 합계 계산

total_row = len(employee_data) + 1

worksheet.write(total_row, 0, "합계", workbook.add_format({"bold": True}))

worksheet.write_formula(total_row, 4, f"=SUM(E2:E{total_row})", money_format)

# 요약 시트에 데이터 추가

summary_sheet.write(
    0,
    0,
    "부서별 인원 및 평균 연봉",
    workbook.add_format({"bold": True, "font_size": 14}),
)

summary_sheet.write(2, 0, "부서", header_format)
summary_sheet.write(2, 1, "인원수", header_format)
summary_sheet.write(2, 2, "평균 연봉", header_format)

# 부서 목록 (중복 제거)

departments = list(set([emp[2] for emp in employee_data]))

# 부서별 통계 계산 및 기록

for i, dept in enumerate(departments):

    row = i + 3

    dept_employees = [emp for emp in employee_data if emp[2] == dept]

    count = len(dept_employees)

    avg_salary = sum([emp[4] for emp in dept_employees]) / count

    summary_sheet.write(row, 0, dept, border_format)

    summary_sheet.write(row, 1, count, border_format)

    summary_sheet.write(row, 2, avg_salary, money_format)

# 차트 추가

chart = workbook.add_chart({"type": "column"})

chart.add_series(

    {

        "name": "평균 연봉",

        "categories": ["요약", 3, 0, 3 + len(departments) - 1, 0],

        "values": ["요약", 3, 2, 3 + len(departments) - 1, 2],

        "data_labels": {"value": True, "num_format": "#,##0"},

    }

)

chart.set_title({"name": "부서별 평균 연봉"})
chart.set_x_axis({"name": "부서"})
chart.set_y_axis({"name": "연봉(원)"})
summary_sheet.insert_chart("E3", chart, {"x_scale": 1.5, "y_scale": 1.5})

# 엑셀 파일 저장

workbook.close()
print("employee_details.xlsx 파일이 c:/test 폴더에 생성되었습니다.")
```

- [실습 4] server.py 코드 작성

```
from mcp.server.fastmcp import FastMCP

# MCP 서버 생성
mcp = FastMCP(name="server")


@mcp.tool()
def read_excel(file_name: str) -> list:
    """
    c:/test/ 아래의 엑셀 파일을 읽어 데이터를 리스트로 반환합니다.

    Parameters
    ----------
    file_name : str
        읽을 엑셀 파일의 이름
        예: 'data.xlsx'

    Returns
    -------
    list
        엑셀 데이터가 포함된 딕셔너리 리스트
        예: [{'name': '김철수', 'age': 30}, {...}]
    """
    import os
    import pandas as pd

    # pandas와 openpyxl 라이브러리 필요
    # pip install pandas openpyxl

    file_path = os.path.join("c:/test", file_name)

    try:
        # 엑셀 파일이 존재하는지 확인
        if not os.path.exists(file_path):
            return [f"파일 '{file_name}'는 존재하지 않습니다."]

        # 엑셀 파일 읽기
        df = pd.read_excel(file_path)

        # 데이터프레임을 딕셔너리 리스트로 변환
        result = df.to_dict("records")

        return result
    except Exception as e:
        return [f"파일 '{file_name}'를 읽는 중 오류가 발생했습니다: {e}"]


@mcp.tool()
def write_excel(contents: list, file_name: str = "test.xlsx") -> str:
    """
    리스트를 엑셀 파일로 저장합니다.

    Parameters
    ----------
    contents : list
        딕셔너리 리스트 형태의 데이터
        예: [{'name': '김철수', 'age': 30}, {...}]
    file_name : str, optional
        저장할 엑셀 파일의 이름, 기본값은 'test.xlsx'

    Returns
    -------
    str
        파일 생성 완료 메시지
    """
    import os
    import pandas as pd

    file_path = os.path.join("c:/test", file_name)

    try:
        # 딕셔너리 리스트를 데이터프레임으로 변환
        df = pd.DataFrame(contents)

        # 엑셀 파일로 저장
        df.to_excel(file_path, index=False)

        return f"엑셀 파일 '{file_path}'가 성공적으로 생성되었습니다."
    except Exception as e:
        return f"엑셀 파일 생성 중 오류가 발생했습니다: {str(e)}"


@mcp.tool()
def create_excel_with_formatting(
    contents: list, file_name: str = "formatted.xlsx"
) -> str:
    """
    리스트를 서식이 지정된 엑셀 파일로 저장합니다.

    Parameters
    ----------
    contents : list
        딕셔너리 리스트 형태의 데이터
        예: [{'name': '김철수', 'age': 30}, {...}]
    file_name : str, optional
        저장할 엑셀 파일의 이름, 기본값은 'formatted.xlsx'

    Returns
    -------
    str
        파일 생성 완료 메시지
    """
    import os
    import xlsxwriter

    file_path = os.path.join("c:/test", file_name)

    try:
        # 엑셀 워크북 생성
        workbook = xlsxwriter.Workbook(file_path)

        # 워크시트 추가
        worksheet = workbook.add_worksheet("Data")

        # 헤더 스타일 정의
        header_format = workbook.add_format(
            {
                "bold": True,
                "font_color": "white",
                "bg_color": "#4F81BD",
                "align": "center",
                "valign": "vcenter",
                "border": 1,
            }
        )

        # 데이터 스타일 정의
        data_format = workbook.add_format({"border": 1})

        # 헤더가 있는지 확인
        if contents and len(contents) > 0:
            # 헤더 작성
            headers = list(contents[0].keys())
            for col_idx, header in enumerate(headers):
                worksheet.write(0, col_idx, header, header_format)

            # 데이터 작성
            for row_idx, row_data in enumerate(contents):
                for col_idx, key in enumerate(headers):
                    worksheet.write(
                        row_idx + 1, col_idx, row_data.get(key, ""), data_format
                    )

            # 열 너비 자동 조정
            for col_idx, _ in enumerate(headers):
                worksheet.set_column(col_idx, col_idx, 15)

        # 워크북 닫기
        workbook.close()

        return f"서식이 지정된 엑셀 파일 '{file_path}'가 성공적으로 생성되었습니다."
    except Exception as e:
        return f"서식이 지정된 엑셀 파일 생성 중 오류가 발생했습니다: {str(e)}"


@mcp.tool()
def append_to_excel(file_name: str, new_data: list) -> str:
    """
    기존 엑셀 파일에 새로운 데이터를 추가합니다.

    Parameters
    ----------
    file_name : str
        데이터를 추가할 엑셀 파일 이름
    new_data : list
        추가할 데이터가 포함된 딕셔너리 리스트
        예: [{'name': '홍길동', 'age': 25}, {...}]

    Returns
    -------
    str
        데이터 추가 결과 메시지
    """
    import os
    import pandas as pd

    file_path = os.path.join("c:/test", file_name)

    try:
        # 파일이 존재하는지 확인
        if not os.path.exists(file_path):
            return f"파일 '{file_name}'이 존재하지 않습니다."

        # 기존 엑셀 파일 읽기
        existing_df = pd.read_excel(file_path)

        # 새 데이터를 데이터프레임으로 변환
        new_df = pd.DataFrame(new_data)

        # 두 데이터프레임 병합
        combined_df = pd.concat([existing_df, new_df], ignore_index=True)

        # 병합된 데이터프레임을 다시 엑셀로 저장
        combined_df.to_excel(file_path, index=False)

        return f"엑셀 파일 '{file_name}'에 새 데이터가 성공적으로 추가되었습니다."
    except Exception as e:
        return f"엑셀 파일에 데이터 추가 중 오류가 발생했습니다: {str(e)}"


# 서버 실행
if __name__ == "__main__":
    mcp.run()
```

