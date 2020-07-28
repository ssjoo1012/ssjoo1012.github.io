---
title: "우분투 배경화면 슬라이드쇼"
excerpt: "사이드 프로젝트"

categories:
 - Blog
tags:
 - Blog
permalink: /ubuntu_wallpaper/
last_modified_at: 2020.07.28
---

# 프로젝트 목표
1. 디렉토리 내 이미지들로 배경이 설정됨.
2. 이미지 전환 시간 조절 가능.
3. 디렉토리의 이미지를 추가,삭제해도 정상동작.

# 프로그램 동작 과정
## init
* wallpaper 디렉토리 리스트 저장.  
* 리스트 정렬.  
* 슬라이드쇼 시간 설정.  

## check_list
* wallpaper 디렉토리 변화(파일 삽입 또는 삭제)가 있는지 확인.
```python
	image_list_temp = os.listdir(image_path)
	image_list_temp.sort() #list 생성 및 정렬

	if image_list != image_list_temp: #add or delete image file
		image_list = image_list_temp #replace image_list
		image_list_index = 0 #restart slide show
```
  
## switch_wallpaper
* 리스트 내 이미지 파일로 배경화면 변경.  
```python
	os.system("gsettings set org.gnome.desktop.background picture-uri file:///home/ssj/Pictures/wallpaper/"+image_list[image_list_index])
```
* image_list_index 증가
```python
	image_list_index += 1
	if image_list_index>=len(image_list):
		image_list_index = 0
```
* threading.Timer()를 사용하여 switch_wallpaper()가 계속 호출되도록 함.
```python
	threading.Timer(60*change_term,switch_wallpaper).start()
```
