Vim 정규식 완벽 가이드
Vim에서의 정규식(Regular Expression)은 텍스트 검색, 치환, 패턴 매칭에 매우 강력한 도구입니다. 이 가이드에서는 Vim 정규식의 모든 기능과 실제 사용 사례를 다룹니다.
1. Vim 정규식 기본 설정
Magic 모드 설정
Vim은 4가지 magic 레벨을 제공합니다:
vim" very magic (\v) - 가장 현대적인 정규식 문법
/\v패턴

" magic (\m) - 기본 설정
/\m패턴

" nomagic (\M) - 대부분의 문자가 리터럴
/\M패턴

" very nomagic (\V) - 거의 모든 문자가 리터럴
/\V패턴
2. 기본 메타문자
위치 지정자
vim^ - 줄의 시작
$ - 줄의 끝
\< - 단어의 시작
\> - 단어의 끝

" 예시
/^Chapter     " Chapter로 시작하는 줄
/end\.$       " end.로 끝나는 줄
/\<word\>     " 정확히 'word'만 매치
문자 클래스
vim. - 모든 문자 (줄바꿈 제외)
\s - 공백 문자
\S - 공백이 아닌 문자
\d - 숫자
\D - 숫자가 아닌 문자
\w - 단어 구성 문자 (알파벳, 숫자, _)
\W - 단어 구성 문자가 아닌 것
\a - 알파벳
\A - 알파벳이 아닌 문자
\l - 소문자
\L - 소문자가 아닌 문자
\u - 대문자
\U - 대문자가 아닌 문자

" 예시
/\d\{4}       " 4자리 숫자
/\s\+         " 하나 이상의 공백
/\w\+         " 하나 이상의 단어 문자
3. 수량자
vim* - 0개 이상
\+ - 1개 이상  
\? - 0개 또는 1개
\{n} - 정확히 n개
\{n,} - n개 이상
\{n,m} - n개 이상 m개 이하
\{-} - 최소 매칭 (non-greedy)

" 예시
/ab*          " a 다음에 0개 이상의 b
/ab\+         " a 다음에 1개 이상의 b
/colou\?r     " color 또는 colour
/\d\{3,5}     " 3~5자리 숫자
/.*\{-}       " 최소 매칭
4. 그룹과 역참조
vim\(...\) - 그룹화
\1, \2, ... - 역참조

" 예시
/\(hello\) \1     " hello hello 매치
/\(\d\+\)-\1      " 123-123 같은 패턴
:%s/\(.*\): \(.*\)/\2: \1/   " 콜론 기준으로 위치 바꾸기
5. 문자 범위와 집합
vim[abc] - a, b, 또는 c
[^abc] - a, b, c가 아닌 문자
[a-z] - a부터 z까지
[0-9] - 0부터 9까지

" 예시
/[aeiou]      " 모음
/[^aeiou]     " 자음
/[0-9A-Fa-f]  " 16진수 문자
/[가-힣]      " 한글 문자
6. 선택과 분기
vim\| - OR 연산자

" 예시
/cat\|dog     " cat 또는 dog
/\(Mr\|Mrs\|Ms\)\. Smith    " Mr. Smith, Mrs. Smith, Ms. Smith
7. 특수 이스케이프 시퀀스
vim\n - 줄바꿈
\t - 탭
\r - 캐리지 리턴
\e - 이스케이프
\b - 백스페이스

" 예시
:%s/\t/    /g    " 모든 탭을 4개 공백으로 치환
:%s/\n\{2,}/\r\r/g    " 여러 줄바꿈을 2개로 정규화
8. Look-around (전후방 탐색)
vim\@= - Positive lookahead
\@! - Negative lookahead  
\@<= - Positive lookbehind
\@<! - Negative lookbehind

" 예시
/foo\(bar\)\@=    " bar가 뒤따르는 foo
/foo\(bar\)\@!    " bar가 뒤따르지 않는 foo
/\(foo\)\@<=bar   " foo가 앞에 있는 bar
/\(foo\)\@<!bar   " foo가 앞에 없는 bar
9. 실전 예제 - 검색
기본 검색
vim" 대소문자 구분 없이 검색
/\cpattern
/pattern\c

" 정확한 대소문자로만 검색
/\Cpattern

" 단어 경계 검색
/\<word\>

" 여러 단어 중 하나 검색
/\v(apple|orange|banana)
복잡한 검색 패턴
vim" 이메일 주소 검색
/\v[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}

" URL 검색
/\vhttps?://[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}(/[^[:space:]]*)?

" IP 주소 검색
/\v\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}

" 전화번호 검색 (한국)
/\v0\d{1,2}-\d{3,4}-\d{4}

" HTML 태그 검색
/\v\<[^>]+\>

" 주석 검색 (C 스타일)
/\v\/\*.*\*\/|\/\/.*$
10. 실전 예제 - 치환
기본 치환
vim" 모든 foo를 bar로 치환
:%s/foo/bar/g

" 현재 줄에서만 치환
:s/foo/bar/g

" 특정 범위에서 치환
:10,20s/foo/bar/g

" 시각 모드에서 선택한 영역에서 치환
:'<,'>s/foo/bar/g
고급 치환 패턴
vim" camelCase를 snake_case로 변환
:%s/\v([a-z])([A-Z])/\1_\l\2/g

" snake_case를 camelCase로 변환  
:%s/\v_([a-z])/\u\1/g

" 날짜 형식 변경 (YYYY-MM-DD → MM/DD/YYYY)
:%s/\v(\d{4})-(\d{2})-(\d{2})/\2\/\3\/\1/g

" HTML 태그 제거
:%s/<[^>]*>//g

" 중복된 단어 제거
:%s/\v(\w+)\s+\1/\1/g

" 줄 끝의 공백 제거
:%s/\s\+$//g

" 빈 줄 제거
:g/^$/d

" 주석 제거
:%s/\/\/.*$//g
조건부 치환
vim" 특정 패턴을 포함하는 줄에서만 치환
:g/pattern/s/foo/bar/g

" 특정 패턴을 포함하지 않는 줄에서만 치환
:v/pattern/s/foo/bar/g

" 대화형 치환 (각 항목마다 확인)
:%s/foo/bar/gc
11. 특수 치환 기법
역참조를 이용한 치환
vim" 함수 호출 형식 변경
:%s/\v(\w+)\(([^)]*)\)/\2->\1()/g

" CSV 컬럼 순서 변경
:%s/\v^([^,]*),([^,]*),([^,]*)/\3,\1,\2/

" 인용구 스타일 변경
:%s/"\([^"]*\)"/'\1'/g
계산을 포함한 치환
vim" 숫자 증가
:%s/\v\d+/\=submatch(0)+1/g

" 줄 번호 추가
:%s/^/\=line('.').' '/

" 문자열 길이로 치환
:%s/\v\w+/\=len(submatch(0))/g
12. 실전 활용 예제
코드 리팩토링
vim" 변수명 변경 (전체 단어만)
:%s/\<oldName\>/newName/g

" getter/setter 생성
:%s/\v^\s*private\s+(\w+)\s+(\w+);/&\r\rpublic \1 get\u\2() { return \2; }\r\rpublic void set\u\2(\1 \2) { this.\2 = \2; }/g

" import 문 정렬
:g/^import/m0
로그 파일 분석
vim" 특정 시간대의 로그만 추출
:v/\v\d{2}:[0-5]\d:[0-5]\d/d

" 에러 로그만 추출
:v/ERROR\|FATAL/d

" IP 주소별 요약
:%s/\v.*\s(\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})\s.*/\1/
:sort u
마크다운 문서 처리
vim" 헤더 레벨 증가
:%s/^#/##/

" 리스트 스타일 변경 (*를 -로)
:%s/^\*/-/

" 코드 블록 언어 추가
:%s/^```$/```python/
13. 매크로와 정규식 결합
vim" 매크로 녹화 (q를 눌러 시작)
qa                          " a 레지스터에 매크로 녹화 시작
/\v^\s*function\s+(\w+)    " 함수 찾기
yiw                        " 함수명 복사
o// TODO: Document \<C-r>"  " 주석 추가
q                          " 녹화 종료

" 매크로 실행
@a                         " 한 번 실행
10@a                       " 10번 실행
14. 정규식 디버깅 팁
vim" 매치되는 부분 하이라이트
:set hlsearch

" 매치 개수 확인
:%s/pattern//gn

" 점진적 검색
:set incsearch

" 매치 미리보기
/\v(패턴){-}
15. 성능 최적화
vim" very magic 모드 사용 (성능 향상)
/\v패턴

" 최소 매칭 사용
/.*\{-}end

" 전체 파일 대신 범위 지정
:10,100s/pattern/replace/g

" 복잡한 패턴은 여러 단계로 분리
:%s/step1/temp/g
:%s/temp/step2/g
이 가이드는 Vim 정규식의 모든 주요 기능을 다루었습니다. 실제 사용시에는 상황에 맞는 적절한 패턴을 선택하여 활용하시기 바랍니다. 정규식은 강력하지만 복잡할 수 있으므로, 간단한 패턴부터 시작하여 점진적으로 복잡한 패턴을 만들어가는 것이 좋습니다.