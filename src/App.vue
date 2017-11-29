<template>
    <div>
    <form @submit.prevent="addTodo">
        <div class="form-group">
            <label for="todo">Todo</label>
            <input type="text"
                   class="form-control"
                   id="todo"
                   v-model="title"
                   :placeholder= "placeholderText">
        </div>

        <button type="Envoyer"
                class="btn btn-default">Submit</button>
    </form>

    <div class="row">
        <ul v-for="todo in todos">
            <li>{{ todo.title }}</li>
        </ul>
    </div>
    </div>
</template>

<script>




export default {
    data () {
        return {
            placeholderText: "What's you need to do",
            title: '',
            todos: []
        }
    },
    created: function() {
        var vm = this
        
        this.$http.get('/task/')
        .then(function (response) {
            response.data.data.map(function(todo) {
                this.todos.push({ title: todo.attributes.title })
            }, vm)
        })
        .catch(function (error) {
            console.log(error);
        });
    },
    methods: {
        addTodo: function (event) {
            var vm = this
            this.$http.post('/task/', {
                    "data": {
                        "type": "task",
                        "attributes": {
                            "title": this.title,
                            "done": false
                        }
                    }
                
            })
            .then(function(response) {
                vm.todos.push({ title: vm.title})
                vm.title = ""
            })
            .catch(function(error) { console.log(error)})
        }
    }
}

</script>