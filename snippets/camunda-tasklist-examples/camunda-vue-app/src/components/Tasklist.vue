<template>
  <div>
    <sui-grid>
      <sui-grid-row :columns="2">
        <sui-grid-column :width="5">
          Created
          <sui-segment >
            <sui-list divided relaxed v-if="tasks && tasks.length">
              <sui-list-item v-for="task of tasks" :key="task">
                <sui-list-content>
                  <p><strong>
                    <router-link :to="`/tasklist/${task.id}`">{{task.name}}</router-link><br>
                    <p>My Process</p>
                    {{task.assignee}}
                    {{task.created}}
                    <p>Created 1 hour ago 50</p>
                  </strong></p>
                </sui-list-content>
              </sui-list-item>
            </sui-list>
          </sui-segment>
        </sui-grid-column>
        <sui-grid-column :width="11">
          <sui-segment raised>
            <h2> Approve Data</h2>
            <h2> MyData</h2>
            
            <sui-menu :widths="4">
            <sui-list-item>
              <sui-list-icon name="calendar alternate" />
              <sui-list-content> Set follow-up date</sui-list-content>
            </sui-list-item>
            <sui-list-item>
              <sui-list-icon name="bell" />
              <sui-list-content> Due date</sui-list-content>
            </sui-list-item>
            <sui-list-item icon="th" content="Add groups"></sui-list-item>
            <sui-list-item icon="user" content="Claim"></sui-list-item>
            </sui-menu>

            <sui-menu>
            <sui-menu-item>Form</sui-menu-item>
            <sui-menu-item>History</sui-menu-item>
            <sui-menu-item active>Diagrams</sui-menu-item>
            <sui-menu-item>Description</sui-menu-item>
            </sui-menu>

            <generic-form v-if="this.$route.params.taskId" :taskId="this.$route.params.taskId" :formKey="taskFormKey"></generic-form>
            <div v-if="!this.$route.params.taskId">
              <p>Please choose task.</p>
            </div>
          </sui-segment>
        </sui-grid-column>
      </sui-grid-row>
    </sui-grid>
  </div>
</template>

<script>
  import CamundaRest from '../services/camunda-rest';
  import GenericForm from './GenericForm';

  export default {
    data() {
      return {
        tasks: [],
        taskFormKey: ''
      };
    },
    components: {
      'generic-form': GenericForm
    },
    watch: {
      '$route': 'fetchData'
    },
    methods: {
      fetchData() {
        CamundaRest.getTasks().then((result) => {
          this.tasks = result.data;
        });
        if (this.$route.params.taskId) {
          CamundaRest.getTaskFormKey(this.$route.params.taskId).then((result) => {
            this.taskFormKey = result.data.key;
          });
        }
      }
    },
    mounted() {
      CamundaRest.getTasks().then((result) => {
        this.tasks = result.data;
      });
      this.fetchData();
    }
  };

</script>

<style>
  #ul_top_hypers li {
    display: inline;
}
</style>