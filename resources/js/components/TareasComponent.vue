<template>
    <div>
        <form-notas-component :editando="editando" :nota="nota" @notaAgregada="notaAgregada" @notaEditada="notaEditada"/>

        <h3 class="mt-5">Mis notas</h3>
        <ul class="list-group">
         <li class="list-group-item" v-for="(item, index) in notas" :key="index">
          <span class="badge badge-primary float-right">
            {{item.updated_at}}
          </span>
          <p>{{item.nombre}}</p>
          <p>{{item.descripcion}}</p>
          <p>
            <button class="btn btn-warning btn-sm" 
                @click="editarNota(item)">Editar</button>
            <button class="btn btn-danger btn-sm" 
                @click="eliminarNota(item, index)">Eliminar</button>
          </p>
        </li>
    </ul>
    </div>
</template>

<script>
import FormNotasComponent from './FormNotasComponent.vue';
export default {
  components: { FormNotasComponent },
    data(){
        return{
            notas : [],
            editando: false,
            nota: {},
        }
    },
    created(){
        axios.get('/notas').then(res =>{
            this.notas = res.data;
        });
    },
    methods:{
        notaAgregada(nota){
            this.notas.push(nota);
        },
        notaEditada(nota){
            this.editando=false;
            this.nota = {nombre: '', descripcion: ''};
            if(nota){
                const index = this.notas.findIndex(item => item.id == nota.id);
                this.notas[index] = nota;
            }
        },
        eliminarNota(nota, index){
        const confirmacion = confirm(`Eliminar nota: ${nota.nombre}`);
        if(confirmacion){
            axios.delete(`/notas/${nota.id}`).then(()=>{
                this.notas.splice(index, 1);
            })
        }
        },
        editarNota(nota){
            this.editando=true;
            this.nota.id=nota.id;
            this.nota.nombre=nota.nombre;
            this.nota.descripcion=nota.descripcion;
        },
    }
}
</script>
