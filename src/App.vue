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
      <q-tabs
        v-model="filesbar_tab"
        align="left"
        dense
        no-caps
        style="border-bottom: 1px solid rgba(0, 0, 0, 0.12)"
      >
        <q-tab name="mails" label="Page One" />
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
      </q-tab-panels>
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

// 定义两个变量，分别表示当前的选项卡，以及侧边栏是否展开
const sidebar_tab = ref('files');
const splitterModel = ref(0);
const splitterModelShadow = ref(0);

const filesbar_tab = ref('1');
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
</style>
