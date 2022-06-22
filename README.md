# 알고리즘 설명
vue-grid-layout에서 item을 신규로 생성할 때
위에서부터 아래로, 아래서부터 위로 좌표를 순차 탐색하여
빈 영역을 찾아 신규아이템의 좌표값을 설정하는 알고리즘입니다.

신규로 추가하고자 하는 아이템의 width값과 height값을 반영하기 위해
최소/최대 x좌표, 최소/최대 y좌표값을 구하여 빈 영역의 좌표값을 탐색합니다.


# 함수설명

/** findNewItemPos
* 기능 : 아이템 추가시 위치를 구해내는 알고리즘.
* - 좌에서 우, 상에서 하 순서로 우선 탐색함.
* - Y축이 겹치는 대상을 찾은 후 X축이 겹치지 않을 경우를 최종 좌표값으로 지정한다.
*/

/** yConditionCheck
* 기능 : y축 조건 탐색 함수
* - 특정 조건을 만족하는 아이템 배열을 리턴한다.
*/

/** xConditionCheck
* 기능 : x축 조건 탐색 함수
* - 특정 조건 만족 여부를 Boolean 타입으로 리턴한다.
*/

# 개발하게된 계기

npmjs 사이트 기준  popularity, quality, and maintenance이 모두 높은 라이브러리였으나, 
공식 문서에서 조차 아이템 추가에 대해 고정 좌표로만 추가하는 기능이 전부였다.
때문에 코어 기능인 그리드와 아이템 조작의 퀄리티가 만족스럽지 못했다.
(아이템 추가시 기존 아이템 좌표값 밀림현상 발생 등)




# Engish description
This is an algorithm developed to create new items in the empty space of the vue-grid-layout.
This detects from top to bottom, left to right to discover the x and y coordinates of an empty space.
