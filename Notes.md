 *
 Hac*k
 H
$%^&*&*

Re. Compile (pattern, 
re.compile(pattern, flags) 정규 표현식 패턴을 컴파일
패턴 객체를 반환한다. 만들어진 패턴 객체는 변수에 저장하여 사용

re.search(pattern, string, flags) 문자열 내에서 패턴에 처음으로 매치하는 문자여릉ㄹ 검색한다. 
return if there is any string that is matching, otherwise return none.

re.match(pattern, string, flags), from the starting point of the string, search the string that matches the pattern. use the group() method from the string, returning the string from the object.

re.fullmatch(pattern, string, flags), check if the entire string mtaches the pattern. 

re.findall(pattern, string, flags), 
re.finditer(pattern, string, flags)
search all the strings that match the pattern in the string. 

import re
result = re.findall(‘[A-Z]’, ‘Hello, DreamHack!’)


pattern = re.compile(‘[A-Z]’)
result = pattern.findall(‘Hello, DreamHack!’)

print(result)

pattern.exec(string)
pattern.test(string)
string.match(pattern)

패턴에 처음으로 매치하는 문자열의 일치 정보를 나타내느 결과 배열을 반환한다.

pattern.test(string) returns  true if there is any match from the pattern, false otherwise

string.match(pattern) 