<template>
  <div class="m-10">
    <div class="title-bar">
      <div class="title-label">
        My tasks
      </div>

      <div class="title-select">
        <Select v-model="filter" name="filter" :options="filterOptions" />
      </div>
    </div>

    <ul class="list">
      <li v-for="(task, index) in tasks" :key="index" class="item">
        <TaskItem
          :title="task.title"
          :description="task.description"
          :status-label="task.status.label"
          :status-value="task.status.value"
        />
      </li>
    </ul>
  </div>
</template>

<script>
import TaskItem from '~/components/tasks/TaskItem'
import Select from '~/components/form/Select'

export default {
  components: { Select, TaskItem },

  async asyncData ({ $api, error }) {
    const [err, tasks] = await $api.tasks.findAll()

    if (err) {
      return error({
        statusCode: 404,
        message: err
      })
    }

    return {
      tasks
    }
  },
  data () {
    return {
      filter: ''
    }
  },
  watch: {
    async filter () {
      const [, tasks] = await this.$api.tasks.findAll({ status: this.filter || undefined })
      this.tasks = tasks
    }
  },
  computed: {
    filterOptions () {
      return [
        { value: '', label: 'Toutes' },
        { value: 'DONE', label: 'Done' },
        { value: 'TODO', label: 'To Do' }
      ]
    }
  }
}
</script>

<style lang="scss" scoped>
.title-bar {
  display: flex;
  width: 100%;
  align-items: center;
  justify-content: space-between;
  margin: 60px 0 24px;

  .title-label {
    font-family: Inter;
    font-size: 18px;
    font-weight: bold;
    line-height: 24px;
  }

  .filter-select {

  }
}

.list {
  border-radius: 12px;
  background: #fff;
  box-shadow: 0px 1px 3px rgba(0, 0, 0, 0.1), 0px 1px 2px rgba(0, 0, 0, 0.06);

  .item {
    border-bottom: 1px solid #E5E7EB;

    &:last-child {
      border-bottom: 0;
    }
  }
}
</style>
