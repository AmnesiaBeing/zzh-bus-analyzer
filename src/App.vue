<template>
  <header elevated class="q-pl-md row items-center">
    <div class="text-weight-bold electron-only">Logo</div>
    <div class="row q-col-gutter-md">
      <div class="cursor-pointer q-mx-sm" accesskey="F">
        {{ $t('menubar_project') }}
        {{ $q.platform.is.electron ? '(F)' : '' }}
        <q-menu>
          <q-list dense style="min-width: 120px">
            <q-item clickable v-close-popup @click="onFileOpenClick">
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
          <q-table
            flat
            bordered
            title="Signals"
            dense
            :rows="rows"
            :columns="columns"
            row-key="name"
            :selected-rows-label="getSelectedString"
            selection="multiple"
            v-model:selected="selected"
            hide-bottom
          />
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

  <input
    type="file"
    id="fileSelector"
    accept=".dbc, .arxml, .asc, .blf, .pcap, application/vnd.tcpdump.pcap"
    style="display: none"
  />
</template>

<script setup>
import { ref } from 'vue';
import { useI18n } from 'vue-i18n';
const { t } = useI18n();

import * as d3 from 'd3';

import { onMounted, onUnmounted } from 'vue';

const columns = [
  {
    name: 'color',
    align: 'center',
    label: '',
    field: 'color',
  },
  { name: 'name', label: 'Signal Name', field: 'name', sortable: true },
];

const rows = [
  {
    color: '',
    name: 'abc',
  },
  {
    color: '',
    name: 'aaaa',
  },
];

const canvas = ref();

import test_data from './mock/test_d3.js';

function initCanvas() {
  const width = canvas.value.clientWidth;
  const height = canvas.value.clientHeight;
  const marginTop = 20;
  const marginRight = 20;
  const marginBottom = 30;
  const marginLeft = 30;

  // 创建XY轴的范围定义，并将一定定义域映射到指定值域
  const x = d3
    .scaleLinear()
    .domain([0, d3.max(test_data, (d) => d.time)])
    .range([marginLeft, width - marginRight]);
  // 创建X轴，位于底部，且每80单位尺寸绘制一个分割线
  const xAxis = d3.axisBottom(x).ticks(10);

  const y1 = d3
    .scaleLinear()
    .domain([-1, 2])
    .range([height - marginBottom, (height - marginBottom) / 2]);
  const yAxis1 = d3.axisLeft(y1).ticks(4).tickValues([0, 1, 2]);

  const y2 = d3
    .scaleLinear()
    .domain([-1, 2])
    .range([(height - marginBottom) / 2, marginTop]);
  const yAxis2 = d3.axisLeft(y2).ticks(4).tickValues([0, 1, 2]);

  const line1 = d3
    .line()
    .x((d) => x(d.time))
    .y((d) => y1(d.value));

  const line2 = d3
    .line()
    .x((d) => x(d.time))
    .y((d) => y2(d.value));

  // 开始绘图
  const svg = d3
    .select('.canvas')
    .append('svg')
    .attr('viewBox', [0, 0, width, height])
    .attr('width', width)
    .attr('height', height)
    .attr('style', 'max-width: 100%; height: auto;');
  // .call(zoom);

  // 画坐标轴
  svg
    .append('g')
    .attr('class', 'x-axis')
    .attr('transform', `translate(0,${height - marginBottom})`)
    .call(xAxis);

  svg
    .append('g')
    .attr('class', 'y-axis')
    .attr('transform', `translate(${marginLeft},0)`)
    .call(yAxis1)
    .call((g) => g.select('.domain').remove());

  svg
    .append('g')
    .attr('class', 'y-axis')
    .attr('transform', `translate(${marginLeft},0)`)
    .call(yAxis2)
    .call((g) => g.select('.domain').remove());

  svg
    .append('path')
    .attr('fill', 'none')
    .attr('stroke', 'steelblue')
    .attr('stroke-width', 1.5)
    .attr('d', line1(test_data));

  svg
    .append('path')
    .attr('fill', 'none')
    .attr('stroke', 'steelblue')
    .attr('stroke-width', 1.5)
    .attr('d', line2(test_data));
}

onMounted(() => {
  initCanvas();
});

onUnmounted(() => {
  // window.removeEventListener('resize', myChart.resize as () => void);
});

// 定义两个变量，分别表示当前的选项卡，以及侧边栏是否展开
const sidebar_tab = ref('files');
const splitterModel = ref(0);
// const splitterModelShadow = ref(0);

import { Dbc } from 'candied';

// const fileSelector = ref();

// 处理打开文件功能
const onFileOpenClick = async (evt, go) => {
  document.getElementById('fileSelector')?.click();
};
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

.q-tab-panel {
  padding: 0 !important;
}
</style>
