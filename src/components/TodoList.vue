<!-- <template>

<ul>
    <li v-for="todo in todos" :key="todo.id"
     :class="{'checked':todo.completed}"
     @click='completedTodo(todo.id)'>
      {{todo.text}}
      
      <span @click="deleteTodo(todo)" class="close">×</span>
    </li>
  </ul>

</template>

<script lang="ts"> 
import {defineComponent, PropType} from 'vue'

import{TypeTodo} from '../../src/Types/TypeTodo'

        props:{
                todos: {
                       
                }
        }

</script>

<style scoped>


</style> -->