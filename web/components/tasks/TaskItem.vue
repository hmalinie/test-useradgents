<template>
  <div class="task-item">
    <div class="title-bar">
      <p class="status" :class="statusValue.toLowerCase()">
        {{ statusLabel }}
      </p>

      <p class="title">
        {{ title }}
      </p>

      <div class="actions">
        <button type="button" class="edit-btn">
          <svg-icon name="pencil" width="12" height="12" fill="#fff" />
        </button>

        <button type="button" class="delete-btn">
          <svg-icon name="trash" width="12" height="12" fill="#fff" />
        </button>
      </div>
    </div>

    <div class="description" v-html="descriptionHtml"></div>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      required: true
    },
    description: {
      type: String,
      required: true
    },
    statusLabel: {
      type: String,
      required: true
    },
    statusValue: {
      type: String,
      required: true,
      validator: value => ['DONE', 'TODO'].includes(value)
    }
  },
  computed: {
    descriptionHtml () {
      return this.description
    }
  }
}
</script>

<style lang="scss" scoped>
.task-item {
  padding: 16px 24px;
}

.title-bar {
  display: flex;
  width: 100%;
  align-items: center;
  line-height: 16px;

  .status {
    font-size: 12px;
    font-weight: 500;
    padding: 2px 10px;
    border-radius: 16px;

    &.done {
      background-color: #D1FAE5;
      color: #065F46;
    }

    &.todo {
      background-color: #FEE2E2;
      color: #991B1B;
    }
  }

  .title {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    width: 100%;
    padding: 0 8px;
    color: #4F46E5;
    font-size: 14px;
  }

  .actions {
    white-space: nowrap;
  }
}

.description {
  color: #6B7280;
  margin-top: 8px;
  font-size: 14px;
  overflow: hidden;
}

button {
  border: 0;
}

.edit-btn {
  background-color: #4F46E5;
  color: #fff;
  padding: 2px;
  margin-right: 4px;
}

.delete-btn {
  background-color: #DC2626;
  color: #fff;
  padding: 2px;
}
</style>
