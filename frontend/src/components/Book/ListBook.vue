<template lang="html">
  <div class="container">
    <div class="row">
      <div class="col text-left">

        <div class="col-md-12">
          <h2>Listado de libros</h2>
          <b-button size="sm" :to="{ name: 'NewBook' }" variant="success">Nuevo libro</b-button>
        </div>
        <br>
        <div class="col-md-12">
          <b-table striped hover :items="books" :fields="fields">
            <template slot="action" slot-scope="row">
              <b-button size="sm" variant="primary" :to="{ name:'EditBook', params: { bookId: row.item.id } }">
                Editar
              </b-button>
              <b-button size="sm" variant="danger" v-on:click="deleteBook(row.item.id)">
                Eliminar
              </b-button>
            </template>
          </b-table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  export default {
    data() {
      return {
        fields: [
          {key: 'title', label: 'Título'},
          {key: 'description', label: 'Descripción'},
          {key: 'action', label: ''}
        ],
        books: [],
      }
    },
    methods: {
      getBooks () {
        const path = 'http://localhost:8000/api/v1.0/books/'
        axios.get(path).then((response) => {
          this.books = response.data
        })
        .catch((error) => {
          console.log(error)
        })
      },
      deleteBook(bookId) {
        const path = 'http://localhost:8000/api/v1.0/books/'+bookId+'/'
        swal({
          title: "Estas seguro?",
          text: "Una vez eliminado, no podrá recuperar este registro!",
          icon: "warning",
          buttons: true,
          dangerMode: true,
        })
        .then((willDelete) => {
          if (willDelete) {
            axios.delete(path).then((response) => {
              swal("Poof! Su registro ha sido borrado!", {
                icon: "success",
              }).then(function() {
                  location.href = '/books'
              });
            })
            .catch((error) => {
              swal("No es posible eliminar el libro", "", "error")
            })
          } else {
            swal("Su registro está a salvo!");
          }
        });
      }
    },
    created (){
      this.getBooks()
    }
  }
</script>

<style lang="css" scoped></style>
