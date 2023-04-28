<template>
  <div v-if="editor">
    <bubble-menu
      class="bubble-menu"
      :tippy-options="{ duration: 100 }"
      :editor="editor"
    >
      <button
        @click="editor.chain().focus().toggleBold().run()"
        :class="{ 'is-active': editor.isActive('bold') }"
      >
        Bold
      </button>
      <button
        @click="editor.chain().focus().toggleItalic().run()"
        :class="{ 'is-active': editor.isActive('italic') }"
      >
        Italic
      </button>
      <button
        @click="editor.chain().focus().toggleStrike().run()"
        :class="{ 'is-active': editor.isActive('strike') }"
      >
        Strike
      </button>
      <!-- <template v-if="toolbars.includes('font-size')" #overlay>
        <div class="vue-tiptap-editor-header-divider"></div>
        <a-dropdown
          :trigger="['click']"
          class="vue-tiptap-editor-header-font-size"
        >
          <a
            @click="(e) => e.preventDefault()"
            class="ant-dropdown-link d-inline-block"
            style="width: 76px"
          >
            {{ selected_font_size ? selected_font_size + "px" : "Font size" }}
            <a-icon type="down" />
          </a>
          <a-menu class="font-size">
            <a-menu-item
              v-for="item in font_sizes"
              :key="item"
              @click="onChangeFontSize(item)"
            >
              {{ item }}px
            </a-menu-item>
          </a-menu>
        </a-dropdown>
      </template> -->
      <a-dropdown :trigger="['click']">
        <a class="ant-dropdown-link" @click.prevent> Hover me </a>
        <template #overlay>
          <a-menu>
            <a-menu-item v-for="item in font_sizes" :key="item">
              <span>
                {{ item }}
              </span>
            </a-menu-item>
          </a-menu>
        </template>
      </a-dropdown>
    </bubble-menu>
  </div>
  <editor-content :editor="editor" />
</template>

<script>
import { useEditor, EditorContent, BubbleMenu } from "@tiptap/vue-3";
import "ant-design-vue/dist/antd.css";
// import FontSize from './tiptap/FontSize';
import StarterKit from "@tiptap/starter-kit";
import { computed } from "vue";
import { Dropdown, Menu, MenuItem } from "ant-design-vue";

export default {
  components: {
    EditorContent,
    BubbleMenu,
    ADropdown: Dropdown,
    AMenu: Menu,
    AMenuItem: MenuItem,
  },

  setup() {
    const toolbars = ["font-size"];

    const font_sizes = [
      12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29,
      30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40,
    ];
    const heading = [1, 2, 3, 4, 5, 6];
    const onChangeFontSize = (item) => {
      editor.chain().focus().setFontSize(`${item}px`).run();
      if (selected_heading.value != null) {
        editor
          .chain()
          .focus()
          .toggleHeading({ level: selected_heading.value })
          .run();
      }
    };

    const selected_heading = computed(() => {
      let selected = null;
      if (editor) {
        let find = heading.value.find((el) => {
          return editor.isActive("heading", { level: el });
        });
        if (find) {
          return find;
        }
      }
      return selected;
    });

    const editor = useEditor({
      content:
        "<p>A Vue.js wrapper component for tiptap to use <code>v-model</code>.</p>",
      extensions: [StarterKit],
    });
    return { editor, toolbars, font_sizes, heading, onChangeFontSize };
  },
};
</script>

<style lang="scss">
/* Basic editor styles */
.ProseMirror {
  > * + * {
    margin-top: 0.75em;
  }

  ul,
  ol {
    padding: 0 1rem;
  }

  blockquote {
    padding-left: 1rem;
    border-left: 2px solid rgba(#0d0d0d, 0.1);
  }
}

.bubble-menu {
  display: flex;
  background-color: #0d0d0d;
  padding: 0.2rem;
  border-radius: 0.5rem;

  button {
    border: none;
    background: none;
    color: #fff;
    font-size: 0.85rem;
    font-weight: 500;
    padding: 0 0.2rem;
    opacity: 0.6;

    &:hover,
    &.is-active {
      opacity: 1;
    }
  }
}

.floating-menu {
  display: flex;
  background-color: #0d0d0d10;
  padding: 0.2rem;
  border-radius: 0.5rem;

  button {
    border: none;
    background: none;
    font-size: 0.85rem;
    font-weight: 500;
    padding: 0 0.2rem;
    opacity: 0.6;

    &:hover,
    &.is-active {
      opacity: 1;
    }
  }
}
.ant-dropdown-menu {
  max-height: 300px;
  overflow-y: scroll;
}
</style>
