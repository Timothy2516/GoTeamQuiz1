<script>
import { mapActions } from 'vuex'

export default {
  name: 'TodoWrapperActions',
  data() {
    return {
      isLoading: false,
      editMode: false,
      editTitle: ''
    }
  },
  methods: {
    ...mapActions('todos', ['updateTodo', 'removeTodo']),
    async load(funct, args) {
      this.isLoading = true
      await funct(args)
      this.isLoading = false
    },
    async toggleTodo(todo) {
      const params = { completed: !todo.is_done, title: todo.title }
      await this.updateTodo({ params, todo })
    },
    async editTodo(todo) {
      const params = { title: this.editTitle }
      await this.updateTodo({ params, todo })
      this.resetEditMode()
    },
    async deleteTodo(todo) {
      await this.removeTodo(todo)
    },
    async deleteTodoAndBackHome(todo) {
      await this.removeTodo(todo)
      this.backToHome()
    },
    setEditMode(title) {
      this.editTitle = title
      this.editMode = true
    },
    resetEditMode() {
      this.editTitle = ''
      this.editMode = false
    },
    backToHome() {
      this.$router.push('/')
    }
  },
  render() {
    return this.$scopedSlots.default({
      load: (funct, args) => this.load(funct, args),
      todoData: {
        isLoading: this.isLoading,
        editMode: this.editMode
      },
      todoActions: {
        toggle: todo => this.toggleTodo(todo),
        edit: todo => this.editTodo(todo),
        delete: todo => this.deleteTodo(todo),
        deleteAndBackHome: todo => this.deleteTodoAndBackHome(todo)
      },
      todoEditMode: {
        reset: this.resetEditMode,
        set: this.setEditMode
      },
      todoInput: {
        value: { value: this.editTitle },
        event: { input: e => (this.editTitle = e) }
      }
    })
  }
}
</script>

<style scoped></style>
