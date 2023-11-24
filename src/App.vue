<template>
  <q-layout view="hHh LpR fFf">
    <q-header elevated class="topbar q-pl-md row items-center">
      <div class="text-weight-bold electron-only">Logo</div>
      <div class="row q-col-gutter-md">
        <div class="cursor-pointer">
          {{ $t('menubar_project') }}
          {{ $q.platform.is.electron ? '(F)' : '(F)' }}
          <q-menu>
            <q-list dense style="min-width: 100px">
              <q-item clickable v-close-popup>
                <q-item-section>
                  {{ $t('menubar_project_open') }}
                </q-item-section>
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
                    <q-item v-for="n in 3" :key="n" dense clickable>
                      <q-item-section>
                        最近打开的文件（功能待完成）
                      </q-item-section>
                      <q-item-section side>
                        <q-icon name="keyboard_arrow_right" />
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
              </q-item>
            </q-list>
          </q-menu>
        </div>
        <div class="cursor-pointer">
          {{ $t('menubar_help') }}
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
    </q-header>

    <aside
      class="fixed"
      style="
        width: 64px;
        top: 32px;
        bottom: 32px;
        border-right: 1px solid rgba(0, 0, 0, 0.12);
      "
    >
      <q-tabs
        v-model="sidebar_tab"
        vertical
        class="fixed text-teal"
        style="width: 64px"
      >
        <q-tab name="mails" icon="mail" />
        <q-tab name="alarms" icon="alarm" />
        <q-tab name="movies" icon="movie" />
      </q-tabs>
    </aside>

    <q-splitter
      v-model="splitterModel"
      class="fixed"
      style="
        height: calc(100vh - 64px);
        width: calc(100vw - 64px);
        top: 32px;
        bottom: 32px;
        left: 64px;
      "
    >
      <template v-slot:before>
        <q-tab-panels v-model="sidebar_tab" vertical>
          <q-tab-panel name="mails">
            <div class="text-h4 q-mb-md">Mails</div>
            <p>
              Lorem ipsum dolor sit, amet consectetur adipisicing elit. Quis
              praesentium cumque magnam odio iure quidem, quod illum numquam
              possimus obcaecati commodi minima assumenda consectetur culpa fuga
              nulla ullam. In, libero.
            </p>
          </q-tab-panel>

          <q-tab-panel name="alarms">
            <div class="text-h4 q-mb-md">Alarms</div>
            <p>
              Lorem ipsum dolor sit, amet consectetur adipisicing elit. Quis
              praesentium cumque magnam odio iure quidem, quod illum numquam
              possimus obcaecati commodi minima assumenda consectetur culpa fuga
              nulla ullam. In, libero.
            </p>
          </q-tab-panel>

          <q-tab-panel name="movies">
            <div class="text-h4 q-mb-md">Movies</div>
            <p>
              Lorem ipsum dolor sit, amet consectetur adipisicing elit. Quis
              praesentium cumque magnam odio iure quidem, quod illum numquam
              possimus obcaecati commodi minima assumenda consectetur culpa fuga
              nulla ullam. In, libero.
            </p>
          </q-tab-panel>
        </q-tab-panels>
      </template>

      <template v-slot:after>
        <q-tabs
          v-model="sidebar_tab"
          align="left"
          dense
          no-caps
          style="border-bottom: 1px solid rgba(0, 0, 0, 0.12)"
        >
          <q-route-tab name="mails" label="Page One" />
          <q-route-tab name="alarms" label="Page Two" />
          <q-route-tab name="movies" label="Page Three" />
        </q-tabs>
        <q-tab-panels v-model="sidebar_tab">
          <q-tab-panel name="mails">
            <div class="text-h4 q-mb-md">Mails</div>
            <p>
              Lorem ipsum dolor sit, amet consectetur adipisicing elit. Quis
              praesentium cumque magnam odio iure quidem, quod illum numquam
              possimus obcaecati commodi minima assumenda consectetur culpa fuga
              nulla ullam. In, libero.
            </p>
          </q-tab-panel>

          <q-tab-panel name="alarms">
            <div class="text-h4 q-mb-md">Alarms</div>
            <p>
              Lorem ipsum dolor sit, amet consectetur adipisicing elit. Quis
              praesentium cumque magnam odio iure quidem, quod illum numquam
              possimus obcaecati commodi minima assumenda consectetur culpa fuga
              nulla ullam. In, libero.
            </p>
          </q-tab-panel>

          <q-tab-panel name="movies">
            <div class="text-h4 q-mb-md">Movies</div>
            <p>
              Lorem ipsum dolor sit, amet consectetur adipisicing elit. Quis
              praesentium cumque magnam odio iure quidem, quod illum numquam
              possimus obcaecati commodi minima assumenda consectetur culpa fuga
              nulla ullam. In, libero.
            </p>
          </q-tab-panel>
        </q-tab-panels>
      </template>
    </q-splitter>

    <q-footer elevated class="bg-grey-8 text-white">
      <q-bar>
        <q-space />
        <div class="col text-center text-weight-bold">底部状态栏（未完成）</div>
        <q-space />
      </q-bar>
    </q-footer>
  </q-layout>
</template>

<script setup lang="ts">
import { ref } from 'vue';

// TODO: 根据一个Object渲染菜单，Object中定义名称，快捷键，调用的函数！

// 定义两个变量，分别表示当前的选项卡，以及侧边栏是否展开
const sidebar_tab = ref('mails');
const sidebar_drawer = ref(true);

const splitterModel = ref(0);
</script>

<style lang="scss">
$TOPBAR_HEIGHT: 32px;
$TOOLBAR_MENU_PADDING: 5px;

$BOTTOMBAR_HEIGHT: 16px;

$SIDEBAR_WIDTH: 48px;
$SIDEBAR_DRAWER_MIN_WIDTH: 5vw;

.topbar {
  height: $TOPBAR_HEIGHT;
}

.menubar {
}
</style>
