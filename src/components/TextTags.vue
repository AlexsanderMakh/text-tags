<template>
  <div :class="blockClass">
    <v-row class="tags-container" :class="alignmentClass">
      <div v-for="(tag, index) in visibleTags" :key="index" class="tag-wrapper">
        <v-col class="tag-item">
          <v-icon v-if="tag.icon" :class="`${blockClass}__icon`">{{ tag.icon }}</v-icon>
          <span :class="`${blockClass}__text`">{{ tag.text }}</span>
        </v-col>
        <v-col v-if="index < visibleTags.length - 1" class="tag-separator">
          <v-icon :class="`${blockClass}__separator`">mdi-circle-small</v-icon>
        </v-col>
      </div>
    </v-row>
  </div>
</template>

<script>
export default {
  name: "TextTags",
  props: {
    tags: {
      type: Array,
      required: true,
    },
    alignment: {
      type: String,
      default: "left",
      validator: value => ["left", "justify"].includes(value),
    },
  },
  data() {
    return {
      blockClass: "text-tags",
      visibleTags: [],
    };
  },
  computed: {
    alignmentClass() {
      return this.alignment === "left" ? `${this.blockClass}--left` : `${this.blockClass}--justify`;
    },
  },
  mounted() {
    this.calculateVisibleTags();
    window.addEventListener('resize', this.calculateVisibleTags);
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.calculateVisibleTags);
  },
  methods: {
    calculateVisibleTags() {
      this.$nextTick(() => {
        const containerWidth = this.$el.offsetWidth;
        let totalWidth = 0;
        this.visibleTags = [];

        for (let i = 0; i < this.tags.length; i++) {
          const tag = this.tags[i];
          const tagWidth = this.calculateTagWidth(tag);
          if (totalWidth + tagWidth <= containerWidth) {
            this.visibleTags.push(tag);
            totalWidth += tagWidth;
            if (i < this.tags.length - 1) {
              totalWidth += this.calculateSeparatorWidth();
            }
          } else {
            break;
          }
        }
      });
    },
    calculateTagWidth(tag) {
      const tagElement = document.createElement('div');
      tagElement.style.visibility = 'hidden';
      tagElement.style.position = 'absolute';
      tagElement.className = 'tag-item';
      if (tag.icon) {
        const iconElement = document.createElement('v-icon');
        iconElement.innerHTML = tag.icon;
        tagElement.appendChild(iconElement);
      }
      const textElement = document.createElement('span');
      textElement.className = 'tag-text';
      textElement.innerText = tag.text;
      tagElement.appendChild(textElement);
      document.body.appendChild(tagElement);
      const tagWidth = tagElement.offsetWidth;
      document.body.removeChild(tagElement);
      return tagWidth;
    },
    calculateSeparatorWidth() {
      const separatorElement = document.createElement('div');
      separatorElement.style.visibility = 'hidden';
      separatorElement.style.position = 'absolute';
      separatorElement.className = 'tag-separator';
      const iconElement = document.createElement('v-icon');
      iconElement.innerHTML = 'mdi-circle-small';
      separatorElement.appendChild(iconElement);
      document.body.appendChild(separatorElement);
      const separatorWidth = separatorElement.offsetWidth;
      document.body.removeChild(separatorElement);
      return separatorWidth;
    }
  }
};
</script>

<style lang="scss">
.text-tags {
  &--left {
    .tags-container {
      justify-content: flex-start;
    }
  }

  &--justify {
    .tags-container {
      justify-content: space-between;
    }
  }

  .tag-wrapper {
    display: flex;
    align-items: center;
  }

  .tag-item {
    display: flex;
    align-items: center;
    white-space: nowrap;
  }

  .text-tags__icon {
    margin-right: 4px;
  }

  .text-tags__separator {
    margin-left: 8px;
    margin-right: 8px;
  }

  .tag-separator {
    display: flex;
    align-items: center;
    justify-content: center;
  }
}
</style>
