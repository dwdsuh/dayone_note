




## Linux Command Cheat Sheet/ 리눅스 명령어 정리


This blog post is a summary of [Mr. Schafer's youtube video](https://www.youtube.com/playlist?list=PL-osiE80TeTvGhHkpvfmKWOiIPF8UVy6c)



### Mac Terminal Cheat Sheet



- Ctrl+a: 맨 앞으로
- Ctrl+e: 맨 뒤로
- Opt+마우스 클릭: 커서 이동
- Ctrl+u: 커서 앞의 내용 모두 삭제
- Ctrl+k: 커서 뒤의 내용 모두 삭제
- drag and drop folder: 디렉토리 경로 완성




## find commend in linux
+ find .


--> 모든 파일 탐색

+ find . -type d


--> 디렉토리 탐색. 파일 탐색은 -type f

+ find . -type f -name "파일이름"


--> 파일이름에 해당하는 파일 찾기
+ find . type f -iname "파일이름"


--> i의 의미는 capital insensitive

+ find . -type f -mmin -10


--> find every file that was modified less than 10 minutes ago.

+ find . -type f -mmin +1 -mmin -5


--> find every file that was modified more than a minute, less than 5 minutes ago.
cf) amin: accessed minute, cmin: changed minute

+ find . -size +5M


--> 5메가 이상 파일 찾기

+ find . -empty


--> 빈 파일 찾기
+ find . -perm <something>


--> find by permission

+ find some-dir -exec chown <username:www-data> {} +


--> -exec: 뒤의 명령어를 실행하라. chown: 작성자 변경, {}: placeholder, +: 명령어가 끝났음을 표시

+ find <디랙토리명> -type f -name "*.jpg"


--> 디렉토리에서 .jpg파일 모두 찾기. -type d 로 설정하면 디랙토리를 찾는다.

+ find . -type f -name "*.jpg" -maxdepth 1 -exec rm {} 


--> 현재 디렉토리에서 .jpg 파일을 모두 찾는다. 찾은 결과에 대해서 삭제하는 명령을 실행한다. 




## grep command in linux




+ grep: global regular expression print

+ grep "something" target.txt


    --> target.txt에서 something을 포함하는 단어 찾기

+ grep -w "something" target.txt


    --> target.txt에서 something과 정확히 일치하는 단어 찾기

+ grep -wi "something" target.txt


    --> i를 추가하여 case-insensitive하게 해준다. 

+ grep -win "something" target.txt


    --> n을 추가하여 그 단어가 발견된 행을 뽑아준다.

+ grep -win -B 4 "something" target.txt


    --> -B 4를 추가하면 그 단어가 발견된 전(Before) 4문장을 뽑아준다.A를 써주면 그 단어가 발견된 후 4문장을 뽑아준다. C는 앞뒤를 뽑아준다(Context). 

+ grep -win "John Williams" ./*


    --> 해당 단어를 포함하는 파일을 하위 디랙토리에서 찾아준다. 

+ grep -win "John Williams" ./*.txt


    --> 해당 단어를 포함하는 텍스트 파일을 하위 디랙토리에서 찾아준다. 

+ grep -winr "something" ./


    --> r을 추가하면 recursive하므로 하위 디렉토리를 모두 서치해준다. 

+ grep -winl "John Williams" .


    -->  l을 추가하면 해당 단어를 포함하고 있는 파일을 출력해준다. 

+ grep -winc "John Williams" .


    -->  c를 추가하면 count를 해준다. 

+ history


    --> 여태까지 사용한 명령어들이 나온다. 

+ history | grep "git commit"


    --> git commit이 들어간 히스토리 내 명령어를 찾아준다. 

+ history | grep "git commit" | grep "dotfile"


    --> 히스토리 내에서 git commit가 들어갔던 명령어를 찾고 그 결과에서 dotfile이 들어간 명령어들을 찾는다. 


+ grep "...-....-...." target.txt


    --> .은 정규식에서 모든 문자를 나타낸다. 전화번호 형태의 글자를 모두 찾아낸다. 

+ grep "\d{3}-\d{3}-\d{4}" names.txt


    --> 정규식을 썻지만 맥에서 디폴트로 작동하지는 않는다. 


## cURL command

+ curl http://google.com 


    --> return html

+ curl http://google.com/json-test


    --> return json

+ curl -o <file_name> <http address>


    --> save html info of the address as file_name

## rsync

+ rsync is install in mac by default

+ rsync is useful when transfering file. 

+ rsync Original/* Backup/


    --> Sending all files in Original directory to Backup directory

+ rsync -r Original/ Backup/


    --> syncing content of Original directory to Backup directory
+ rsync -r Original Backup/


    --> syncing Original directory itself to Backup directory

+ rsync -a --dry-run Original/ Backup/


    --> -a: archive, --dry-run: 파일이 제대로 전송되는지 알아보는 시도. 실제로 파일이 전송되지는 않음.

+ rsync -av --dry-run Original/ Backup/


    --> -v: verbose, 옮기는 파일들이 화면에 출력된다

+ rsync -av --delete --dry-run Original/ Backup/


    --> Original에 없는 파일들을 Backup/에서 지운다. 주의할 점은 original dir가 빈 폴더면 다 지워진다. 

+ rsync -zaP ~/Projects/my_site remote@192.148.234:~/public


    --> -z: zip the file while senting, -a: archive, -P: show the progress


## Cron Jobs: How to Schedule Commands with crontab(cron table)

+ crontab -e


    --> vi 창을 열어준다. 
    분 시간 데이 달 요일 command_to_execute
eg) * * * * * echo 'hello world' >> /tmp/test.txt


    */2 * * * * echo "hello world" >> /tmp/test.txt 
    
    (2분마다 명령을 실행)


    *9-17 * * 1-5 echo "hello world" >> /tmp/test.txt 
    
    (월요일부터 일요일까지 9시부터 오후 5시까지 명령어를 반복해서 실행)

















   





