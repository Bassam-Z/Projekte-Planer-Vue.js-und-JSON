<template>
  <div class="project" :class="{ complete: project.complete}">
    <div class="action">
        <h3 @click="showDetails = !showDetails">{{ project.title }}</h3>
        <div class="icons">
            <router-link :to="{name: 'EditProject', params: { id: project.id }}">
                <span class="material-symbols-outlined">border_color</span>
            </router-link>
            <span @click="deleteProject" class="material-symbols-outlined">Delete</span>
            <span @click="toggleComplete" class="material-symbols-outlined tick">Done</span>
        </div>
    </div>
    <div v-if="showDetails" >
        <p>{{ project.details }}</p>
    </div>
  </div>
</template>

<script>

export default {
    props:['project'],
    data(){
        return{
            showDetails: false,
            uri:'http://localhost:3000/projects/' + this.project.id
        }
    },
    methods:{
        deleteProject(){
            fetch(this.uri, {method: 'DELETE'})
                .then(() => this.$emit('delete', this.project.id))
                .catch(err => console.log(err))
        },
        toggleComplete(){
            fetch(this.uri, { 
                method: 'PATCH',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({complete: !this.project.complete})
            }).then(() => {
                this.$emit('complete', this.project.id )
            }).catch(err => console.log(err))
        }
    }


}
</script>

<style>
.project{
    margin: 20px auto;
    background: white;
    padding: 10px 20px;
    border-radius: 4px;
    box-shadow: 1px 2px 3px rgb(33, 33, 33);
    border-left: 4px solid #e90074;
}
.project.complete{
    border-left: 4px solid #00e988;
}
.project.complete .tick{
    color: #00e988;
}
h3{
    cursor: pointer;
}
.action{
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.material-symbols-outlined{
    font-size: 24px;
    margin-left: 10px;
    color: #bbb;
    cursor: pointer;
}
.material-symbols-outlined:hover {
    color:#777
}
</style>