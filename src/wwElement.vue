<template>
  <draggable
    v-model="items"
    :itemKey="getItemKey"
    :disabled="isEditing"
    :animation="200"
    :style="{ ...$attrs.style, ...layoutStyle }"
    v-bind="options"
  >
    <template #item="{ element, index }">
      <div>
        <wwLayoutItemContext
          is-repeat
          :data="element"
          :item="null"
          :index="index"
        >
          <wwElement v-bind="content.itemContainer" />
        </wwLayoutItemContext>
      </div>
    </template>
  </draggable>
</template>

<script>
import draggable from "vuedraggable";

export default {
  components: { draggable },
  props: {
    content: { type: Object, required: true },
  },
  emits: ["trigger-event"],
  setup() {
    return {
      layoutStyle: wwLib.useLayoutStyle(),
    };
  },
  computed: {
    items: {
      get() {
        return wwLib.wwCollection.getCollectionData(this.content.data) || [];
      },
      set(value) {
        this.$emit("trigger-event", { name: "update:list", event: { value } });
      },
    },
  },
};
</script>

<style lang="scss" scoped></style>
