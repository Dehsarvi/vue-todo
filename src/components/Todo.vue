<template>
    <div>
                <section class="todoapp">
                    <header class="header">
                        <h1>لیست کارها</h1>
                        <input class="new-todo" v-model="newItem" @keydown.enter="addToItem"
                        autofocus autocomplete="off"
                        placeholder="چه کاری میخواهی انجام بدهی ؟">
                    </header>
                    <section class="main">
                        <input class="toggle-all" type="checkbox">
                        <ul class="todo-list">
                            <li v-for="(item,index) in filteredItems" :key="index" 
                            :class="{completed:item.completed , editing: item == editedItem}">
                            <div class="view">
                                <input class="toggle" type="checkbox"  value="on" v-model="item.completed">
                                <label @dblclick="editItem(item)">{{item.title}}</label>
                                <button class="destroy" @click="removeItem(item)"></button>
                            </div>
                            <input class="edit" v-model="item.title" @keydown.enter="doneEdit(item)" type="text">
                        </li>
                    </ul>
                </section>
                <footer class="footer">
                    <span class="todo-count">
                        <strong>{{itemCount | activeItems}}</strong> 
                    </span>
                    <ul class="filters">
                        <li><a :class="{selected : this.visibility == 'all'}" @click="visibility='all'" >همه</a></li>
                        <li><a :class="{selected : this.visibility == 'active'}"  @click="visibility='active'" >فعال</a></li>
                        <li><a  :class="{selected : this.visibility == 'completed'}"  @click="visibility='completed'" >انجام شده</a></li>
                    </ul>
                    <button class="clear-completed" @click="removeCompleted()">
                        پاک کردن انجام شده ها
                    </button>
                </footer>
    </section>
    </div>
</template>

<script>

let filters ={
    all:(items)=>{
        return items;
    },
    active:(items)=>{
        return items.filter((item)=>{
            return !item.completed;
        })
    },
      completed:(items)=>{
        return items.filter((item)=>{
            return item.completed;
        })
    }
}

export default {
    name:'todo',
    data(){
        return{
            newItem:'',
            items:[],
            editedItem:null,
            visibility:'all'
        }
    },

    methods:{
        addToItem(){
            let subject = this.newItem.trim();
            if(!subject){
                return
            }
            this.items.push({
                title:subject,
                completed:false
            })
            this.newItem = "";

        },
        removeItem(item){
            this.items.splice(this.items.indexOf(item),1);
        },
        removeCompleted(){
            this.items =  filters['active'](this.items);
        },
        editItem(item){
            this.editedItem = item;
        },
        doneEdit(item){
            if(!this.editedItem){
                return
            }
            this.editedItem=null
            if(!item.title){
                this.removeItem(item);
            }
        }

    },
    computed:{
        itemCount(){
            return filters[this.visibility](this.items).length;
        },

        filteredItems(){
            return filters[this.visibility](this.items);
        }
    },
    filters:{
        activeItems(value){
            return value + ' مورد ';
        }
    }
}
</script>

<style>

</style>