---
name: post
description: Write, edit, publish and take down columns (posts) and notices on a clinic homepage that Standardpin builds and runs, through the Pin AI connector. Use when the user asks for 공지, 휴진 안내, 칼럼, 블로그 글, 글 고치기, 글 내리기 or 글에 사진 넣기, e.g. "다음 주 화요일 휴진 공지 올려줘", "임플란트 관리 칼럼 써줘", "그 공지 내려줘".
---
# 홈페이지 글·공지 쓰고 올리기

Pin AI 연결의 tool로 병원 홈페이지에 글(칼럼)과 공지를 씁니다. 사용자가 분명히 다르게 말하면 그 말을 따르되, 게시 전 확인과 의료광고 경고 알림은 빼지 않습니다.

## 순서
1. 글·공지를 쓰기 전에 get_writing_guide로 쓰는 기준(구조·의료광고 금지선·피할 말)을 한 번 봅니다.
2. 글·공지: save_draft로 초안을 쓰거나 고치고(게시 전 점검과 의료광고 표현 경고가 같이 옵니다), 사용자가 내용을 확인하면 publish_draft로 올립니다. 내리기는 unpublish_post. 올리기는 보통 몇 분 걸리니 get_publish_status로 끝을 확인합니다.
3. 글 사진: list_photos로 홈페이지에 이미 있는 병원 사진을 찾아 add_photo로 초안에 넣고, 사용자가 채팅에 올린 사진도 add_photo로 넣습니다(올린 파일을 넘길 수 있는 앱에서). 돌려받은 markdown 줄을 save_draft 본문의 그 자리에 둡니다(쓰던 초안이면 read_post(draftId)로 지금 본문을 읽고 그 줄을 넣어 다시 저장). 치료 전후·환자 얼굴 사진은 넣지 않습니다. 넣을 수 없으면 Space 링크(spaceUrl)에서 넣도록 안내합니다.
4. 게시는 병원 이름으로 공개됩니다. 올리기 전에 제목과 본문을 사용자에게 보여 주고 확인을 받으세요. 경고(advisories)가 있으면 그대로 알리고 고칠지 묻습니다.

## 지킬 것
- 숫자는 tool이 준 값만 말하고, 값이 없거나(null) 비교할 수 없으면(comparable: false) 그렇다고 말합니다. 순위·효과·치료 결과를 보장하는 표현, '최고·1위·유일' 같은 최상급 표현은 쓰지 않습니다.
- 전후 사례(case)는 회원에게만 보이는 사진 사례이고 읽기만 합니다: list_posts·read_post로 보고, 올리거나 고치는 일(사진·환자 동의 확인)은 사람이 Space 글 관리 "새 사례"에서 합니다. save_draft·publish_draft로 사례를 만들지 않습니다.
- 디자인·구조·문구 수정, 새 페이지처럼 글이 아닌 변경은 request_change로 담당자에게 남깁니다(담당자가 반영). 진료시간은 update_clinic_hours가 Space에 저장하고 홈페이지에 반영합니다.
