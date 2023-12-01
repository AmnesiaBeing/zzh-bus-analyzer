<template>
  <header elevated class="q-pl-md row items-center">
    <div class="text-weight-bold electron-only">Logo</div>
    <div class="row q-col-gutter-md">
      <div class="cursor-pointer q-mx-sm" accesskey="F">
        {{ $t('menubar_project') }}
        {{ $q.platform.is.electron ? '(F)' : '' }}
        <q-menu>
          <q-list dense style="min-width: 120px">
            <q-item clickable v-close-popup>
              <q-item-section>
                {{ $t('menubar_project_open') }}
              </q-item-section>
              <q-item-section side>Ctrl+O</q-item-section>
            </q-item>
            <q-item clickable>
              <q-item-section>
                {{ $t('menubar_project_open_recent') }}
              </q-item-section>
              <q-item-section side>
                <q-icon name="keyboard_arrow_right" />
              </q-item-section>

              <q-menu anchor="top end" self="top start">
                <q-list>
                  <!-- <q-item v-for="n in 3" :key="n" dense clickable>
                    <q-item-section>
                      最近打开的文件（功能待完成）
                    </q-item-section>
                  </q-item> -->
                  <q-item dense disable v-close-popup>
                    <q-item-section>
                      {{ $t('menubar_project_open_recent_no_recent_files') }}
                    </q-item-section>
                  </q-item>
                </q-list>
              </q-menu>
            </q-item>

            <q-separator />

            <q-item clickable v-close-popup>
              <q-item-section>
                {{ $t('menubar_project_exit') }}
              </q-item-section>
              <q-item-section side>Ctrl+Q</q-item-section>
            </q-item>
          </q-list>
        </q-menu>
      </div>

      <div class="cursor-pointer q-mr-sm" accesskey="E">
        {{ $t('menubar_edit') }}
        {{ $q.platform.is.electron ? '(E)' : '' }}
        <q-menu>
          <q-list dense style="min-width: 120px">
            <q-item clickable v-close-popup>
              <q-item-section>
                {{ $t('menubar_edit_undo') }}
              </q-item-section>
              <q-item-section side>Ctrl+Z</q-item-section>
            </q-item>
            <q-item clickable v-close-popup>
              <q-item-section>
                {{ $t('menubar_edit_redo') }}
              </q-item-section>
              <q-item-section side>Ctrl+Y</q-item-section>
            </q-item>

            <q-separator />

            <q-item clickable v-close-popup>
              <q-item-section>
                {{ $t('menubar_edit_cut') }}
              </q-item-section>
              <q-item-section side>Ctrl+X</q-item-section>
            </q-item>
            <q-item clickable v-close-popup>
              <q-item-section>
                {{ $t('menubar_edit_copy') }}
              </q-item-section>
              <q-item-section side>Ctrl+C</q-item-section>
            </q-item>
            <q-item clickable v-close-popup>
              <q-item-section>
                {{ $t('menubar_edit_paste') }}
              </q-item-section>
              <q-item-section side>Ctrl+V</q-item-section>
            </q-item>

            <q-separator />

            <q-item clickable v-close-popup>
              <q-item-section>
                {{ $t('menubar_edit_search') }}
              </q-item-section>
              <q-item-section side>Ctrl+F</q-item-section>
            </q-item>
            <q-item clickable v-close-popup>
              <q-item-section>
                {{ $t('menubar_edit_replace') }}
              </q-item-section>
              <q-item-section side>Ctrl+H</q-item-section>
            </q-item>
          </q-list>
        </q-menu>
      </div>

      <div class="cursor-pointer q-mr-sm" accesskey="H">
        {{ $t('menubar_help') }} {{ $q.platform.is.electron ? '(H)' : '' }}
        <q-menu>
          <q-list dense style="min-width: 100px">
            <q-item clickable v-close-popup>
              <q-item-section>
                {{ $t('menubar_help_about') }}
              </q-item-section>
            </q-item>
            <q-item clickable v-close-popup>
              <q-item-section>
                {{ $t('menubar_help_help') }}
              </q-item-section>
              <q-item-section side>F1</q-item-section>
            </q-item>
          </q-list>
        </q-menu>
      </div>
    </div>
    <q-space />
    <div class="col text-center text-weight-bold">
      {{ '当前选择文件名（功能待完成）' }} - {{ $t('application') }}
    </div>
    <q-space />
    <div class="electron-only">
      <q-btn dense flat icon="minimize" />
      <q-btn dense flat icon="crop_square" />
      <q-btn dense flat icon="close" />
    </div>
  </header>

  <aside>
    <q-tabs v-model="sidebar_tab" vertical class="text-teal">
      <q-tab name="files" icon="description" class="sidebar_icon" />
      <q-tab name="search" icon="search" class="sidebar_icon" />
    </q-tabs>
  </aside>

  <q-splitter
    v-model="splitterModel"
    class="fixed splitter"
    separator-class="splitterbar"
  >
    <template v-slot:before>
      <q-tab-panels v-model="sidebar_tab" vertical>
        <q-tab-panel name="files">
          <div class="text-h4 q-mb-md">Mails</div>
        </q-tab-panel>

        <q-tab-panel name="search">
          <div class="text-h4 q-mb-md">Alarms</div>
        </q-tab-panel>
      </q-tab-panels>
    </template>

    <template v-slot:after>
      <main>
        <div ref="canvas" class="canvas"></div>
      </main>
    </template>
  </q-splitter>

  <footer class="q-pl-md row items-center">
    <q-space />
    <div class="col text-center text-weight-bold">底部状态栏（未完成）</div>
    <q-space />
  </footer>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { useI18n } from 'vue-i18n';
const { t } = useI18n();
import * as echarts from 'echarts';
import { onMounted } from 'vue';
import { onUnmounted } from 'vue';

const canvas = ref();

const testdata = [
  [0, 0],
  [1, 0],
  [2, 0],
  [3, 1],
  [4, 1],
  [5, 0],
];
const xList = testdata.map((item) => item[0]);
const yList = testdata.map((item) => item[1]);
const yList1 = yList.map((v) => v + 1);
const yList2 = yList.map((v) => 1 - v);

const g = 2;
const p = 2;
const n = 6;
const topList = [...new Array(n).keys()].map(
  (v) => p + ((100 - 2 * p + g) / n) * v
);
const gridValue = [...new Array(n).keys()].map((v) => {
  return {
    top: topList[v].toString() + '%',
    bottom: topList[n - v - 1].toString() + '%',
  };
});

let myChart: echarts.ECharts;

onMounted(() => {
  var option = {
    tooltip: {
      trigger: 'axis',
    },
    axisPointer: {
      link: { xAxisIndex: 'all' },
    },
    xAxis: [
      {
        data: xList,
        show: false,
      },
      {
        data: xList,
        show: false,
        gridIndex: 1,
      },
      {
        data: xList,
        name: '时间',
        axisLine: {
          show: true,
          symbol: ['none', 'arrow'],
        },
        gridIndex: 2,
      },
    ],
    yAxis: [
      {
        axisLine: {
          show: true,
          symbol: ['none', 'arrow'],
        },
      },
      {
        axisLine: {
          show: true,
          symbol: ['none', 'arrow'],
        },
        gridIndex: 1,
      },
      {
        axisLine: {
          show: true,
          symbol: ['none', 'arrow'],
        },
        gridIndex: 2,
      },
    ],
    grid: gridValue,
    series: [
      {
        type: 'line',
        step: 'start',
        data: yList,
      },
      {
        type: 'line',
        step: 'start',
        data: yList1,
        xAxisIndex: 1,
        yAxisIndex: 1,
      },
      {
        type: 'line',
        step: 'start',
        data: yList2,
        xAxisIndex: 2,
        yAxisIndex: 2,
      },
    ],
    dataZoom: [
      {
        show: true,
        type: 'slider',
        xAxisIndex: [0, 1, 2],
      },
      {
        type: 'inside', // 支持内部鼠标滚动平移
        startValue: 0,
        endValue: 4,
        zoomOnMouseWheel: false, // 关闭滚轮缩放
        moveOnMouseWheel: true, // 开启滚轮平移
        moveOnMouseMove: true, // 鼠标移动能触发数据窗口平移
        xAxisIndex: [0, 2],
      },
    ],
  };
  myChart = echarts.init(canvas.value);
  myChart.setOption(option);
  window.addEventListener('resize', myChart.resize);
});

onUnmounted(() => {
  window.removeEventListener('resize', myChart.resize);
});

// 定义两个变量，分别表示当前的选项卡，以及侧边栏是否展开
const sidebar_tab = ref('files');
const splitterModel = ref(0);
// const splitterModelShadow = ref(0);
</script>

<style lang="scss">
$TOPBAR_HEIGHT: 32px;
$TOOLBAR_MENU_PADDING: 5px;

$BOTTOMBAR_HEIGHT: 16px;

$SIDEBAR_WIDTH: 64px;
$SIDEBAR_DRAWER_MIN_WIDTH: 5vw;

header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  height: $TOPBAR_HEIGHT;
  background-color: $primary;
  color: white;
  border-bottom: 1px solid rgba(0, 0, 0, 0.12);
}

footer {
  height: $BOTTOMBAR_HEIGHT;
  position: fixed;
  bottom: 0;
  width: 100%;
  font-size: 12px;
  border-top: 1px solid rgba(0, 0, 0, 0.12);
}

aside {
  position: fixed;
  width: $SIDEBAR_WIDTH;
  top: $TOPBAR_HEIGHT;
  bottom: $BOTTOMBAR_HEIGHT;
  border-right: 1px solid rgba(0, 0, 0, 0.12);
}

.sidebar_icon {
  height: $SIDEBAR_WIDTH;
}

.splitter {
  height: calc(100vh - $TOPBAR_HEIGHT - $BOTTOMBAR_HEIGHT);
  width: calc(100vw - $SIDEBAR_WIDTH);
  top: $TOPBAR_HEIGHT;
  bottom: $BOTTOMBAR_HEIGHT;
  left: $SIDEBAR_WIDTH;
}

.splitterbar:active {
  background-color: $primary;
}

.canvas {
  padding: 10px;
  width: 100%;
  height: calc(100vh - $TOPBAR_HEIGHT - $BOTTOMBAR_HEIGHT);
}
</style>
