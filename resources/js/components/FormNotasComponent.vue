<template>
    <div>
        <h3 v-if="!editando">Agregar nota</h3>
        <h3 v-if="editando">Editar nota</h3>
        <form @submit.prevent="validarForm" >
            <input type="text" placeholder="Nombre" class="form-control mb-2" v-model="nota.nombre">
            <input type="text" placeholder="Descripcion" class="form-control mb-2" v-model="nota.descripcion">
            <button v-if="!editando" class="btn btn-primary" type="submit">Agregar</button>
            <button v-if="editando" class="btn btn-success" type="submit">Guardar</button>
            <button v-if="editando" @click="cancelarEdicion()" class="btn btn-danger" type="button">Cancelar</button>
        </form>
    </div>
</template>

<script>
    export default {
        props: ['editando', 'nota'],
        data(){
            return{
                agregando: true,
                // nota : {nombre: '', descripcion: ''},
            }
        },
        methods:{
            validarForm(){
                if(this.nota.nombre.trim() === '' || this.nota.descripcion.trim() === ''){
                    alert('Debes completar todos los campos antes de guardar');
                    return;
                }
                if(this.editando)
                    this.editar();
                else
                    this.agregar();
            },
            agregar(){
                const nota = {
                    nombre : this.nota.nombre, 
                    descripcion : this.nota.descripcion
                };
                this.nota.nombre = '';
                this.nota.descripcion = '';
                axios.post('/notas', nota).then(res => {
                    this.$emit('notaAgregada', res.data);
                });
            },
            editar(){
                console.log("entro al editar axios con id: "+this.nota.id);
                const nota = {
                    nombre : this.nota.nombre, 
                    descripcion : this.nota.descripcion
                };
                axios.put(`/notas/${this.nota.id}`, nota).then(res => {
                    this.$emit('notaEditada', res.data);
                });
            },
            cancelarEdicion(){
                this.$emit('notaEditada', null);
            },
        }
    }
</script>
