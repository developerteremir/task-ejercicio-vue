<template lang="pug">
  #app
    section.section
      .container
      .columns
        .column.is-10.is-offset-1
          nav.panel
            p.panel-heading App de Tareas
              div.panel-block
                .container
                  h3(class="is-size-3") Saludos, {{ name }}
                    form(autocomplete="off")
                    .field
                      label.label.has-text-warning Título de la Tarea
                      .control
                        input(v-model="newTask.title" class="input" type="text" id="title" name="title" autocomplete="off")
                    .field
                      label(class="label has-text-warning") Tiempo de la Tarea
                      .control
                        input(v-model="newTask.time" class="input" type="number" step="1" id="hora" name="hora" )
                    .field
                      .control.buttons.is-right
                        button(@click="addTask" class="button is-info") Enviar
                        button(@click="cancel" class="button is-danger") Cancelar
              div.panel-block
                  .container
                    h5.subtitle.is-5 Horas Trabajadas: {{ totalTime }}
                    table.table.is-fullwidth
                      thead
                        tr
                          th Título
                          th Tiempo
                          th
                      tbody
                        tr(v-for="(task, key) in tasks")
                          td {{ task.title }}
                          td {{ task.time }}
                          td 
                            button(@click="removeTask(key)" class="button is-small is-danger") x     
                        tr(v-show="tasks.length==0", colpsan="3")    
                          td No hay tareas cargadas             
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      name: 'Teresa',
      tasks: [],
      hora: 0,
      newTask: {
        title: '',
        time: 0
      }
    }
  },
  created () {
    this.tasks = JSON.parse(localStorage.getItem('tasks')) || []
  },
  computed: {
    totalTime () {
      const self = this
      self.hora = 0
      this.tasks.forEach(function (task, key) {
        self.hora += parseInt(task.time)
      })
      return self.hora
    }
  },
  methods: {
    addTask () {
      if (!this.newTask.title || !this.newTask.time) { return }
      this.tasks.push({
        title: this.newTask.title,
        time: this.newTask.time
      })

      localStorage.setItem('tasks', JSON.stringify(this.tasks))

      this.newTask.title = ''
      this.newTask.time = 0
    },
    cancel () {
      this.newTask.title = ''
      this.newTask.time = 0
    },
    removeTask (key) {
      return this.tasks.splice(key, 1)
    }
  }
}
</script>

<style lang="scss">
@import './scss/main.scss';
</style>
