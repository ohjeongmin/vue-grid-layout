
<template>
  <div>
    <div class="contents-top-menu">
      <button title="아이템 추가" @click="addNewItem"></button>
    </div>
    <grid-layout
      :layout.sync="items"
      :responsive="false"
      :vertical-compact="false"
      :prevent-collision="true"
      :use-css-transforms="true"
      :col-num="gridColNum"
      :row-height="gridRowNum"
      :is-draggable="editModeYn"
      :is-resizable="editModeYn"
      :auto-size="true"
      :margin="[10, 10]"
      >

      <grid-item :id="item.i"
        v-for="(item, index) in items" :key="item.i"
        tabindex="-1"
        :x="item.x"
        :y="item.y"
        :w="item.w"
        :h="item.h"
        :i="item.i"
        >
      </grid-item>
    </grid-layout>
  </div>
</template>

<script>
import { GridLayout, GridItem } from 'vue-grid-layout'

export default {
  name: 'FindNewPosition',
  components: {
    GridLayout,
    GridItem
  },
  data () {
    return {
      gridColNum: 60, // 그리드 컬럼 분할 수
      gridRowNum: 10, // 로우 확장 단위
      defaultWidth: 20, // 아이템 기본 넓이
      defaultHeight: 15, // 아이템 기본 높이

      defaultItem: { // 기본 아이템 속성
        'i': '',
        'x': null,
        'y': null,
        'w': null,
        'h': null
      },

      items: [] // 아이템 배열
    }
  },

  created () {
  },

  mounted () {
  },

  methods: {
    addNewItem () {
      let childCount = new Date().getTime()
      let resultPos = this.findNewItemPos() // 신규아이템 좌표위치 구하는 알고리즘

      const cp = require('lodash')
      let defaultItem = cp.cloneDeep(this.defaultItem)

      // defaultItem.dsbdId = this.selectDsbdId
      defaultItem.i = String(childCount++)
      defaultItem.x = resultPos.x
      defaultItem.y = resultPos.y
      defaultItem.w = this.defaultWidth
      defaultItem.h = this.defaultHeight

      this.items.push(defaultItem)
    },

    /** findNewItemPos
     * 기능 : 아이템 추가시 위치를 구해내는 알고리즘.
     * - 좌에서 우, 상에서 하 순서로 우선 탐색함.
     * - Y축이 겹치는 대상을 찾은 후 X축이 겹치지 않을 경우를 최종 좌표값으로 지정함.
     */
    findNewItemPos () {
      let maxY = this.gridRowNum * (this.items.length * 10) // y좌표 범위 지정_임의의 큰값
      let xRange = this.gridColNum - this.defaultWidth // x좌표 범위 지정

      // 신규로 생성할 Item값 초기화
      let newItem = {
        minX: 0,
        maxX: 0,
        minY: 0,
        maxY: 0
      }

      // 조건 탐색을 위한 형태로 만들기
      let targetItems = []
      this.items.forEach(item => {
        targetItems.push({
          i: item.i,
          minX: item.x,
          maxX: item.x + item.w,
          minY: item.y,
          maxY: item.y + item.h

        })
      })

      let yPassArr = [] // Y축 조건문에 해당되는 아이템 배열
      let xPassYn = [] // X축 조건문의 통과 여부

      for (let i = 0; i < maxY; i++) {
        for (let j = 0; j <= xRange; j++) {
          newItem.minX = j
          newItem.maxX = j + this.defaultWidth
          newItem.minY = i
          newItem.maxY = i + this.defaultHeight

          yPassArr = this.yConditionCheck(newItem, targetItems)
          if (yPassArr.length === 0) { return { 'x': j, 'y': i } } // y축이 겹치지 않을경우 x축을 비교할 대상이 없으므로 바로 리턴함.
          xPassYn = this.xConditionCheck(newItem, yPassArr)
          if (xPassYn) { return { 'x': j, 'y': i } } else continue
        }
      }
    },

    /** yConditionCheck
     * 기능 : y축 조건 탐색 함수
     * - 특정 조건을 만족하는 아이템 배열을 리턴한다.
    */
    yConditionCheck (newItem, targetItems) {
      let resultArr = []
      resultArr = targetItems.filter(item =>
        (item.minY === newItem.minY && item.maxY === newItem.maxY) ||
        (newItem.minY >= item.minY && newItem.minY < item.maxY) ||
        (newItem.maxY <= item.maxY && newItem.maxY > item.minY)
      )

      return resultArr
    },

    /** xConditionCheck
     * 기능 : x축 조건 탐색 함수
     * - 특정 조건 만족 여부를 Boolean 타입으로 리턴한다.
    */
    xConditionCheck (newItem, yPassArr) {
      /* yPassArr 요소는 하위 두개의 요소중 하나를 반드시 만족해야 한다. */
      for (let i = 0; i < yPassArr.length; i++) {
        if (yPassArr[i].maxX <= newItem.minX) {
          continue
        }
        if (yPassArr[i].minX >= newItem.maxX) {
          continue
        } else {
          return false
        }
      }
      return true
    }
  }
}
</script>

<style lang="scss">

</style>
