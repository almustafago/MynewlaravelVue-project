<template>
  <div class="container">
    <div class="row">
      <div class="col-12">
        <div class="card">
          <div class="card-header">
            <h3 class="card-title">Users Table</h3>

            <div class="card-tools">
            <button  class="btn btn-success" data-toggle="modal" data-target="#examplemodal">
  Add New <i class="fas fa-user-plus"></i>
</button>

            </div>
          </div>
          <!-- /.card-header -->
          <div class="card-body table-responsive p-0">
            <table class="table table-hover text-nowrap">
              <thead>
                <tr>
                  <th>ID</th>
                  <th>User</th>
                  <th>Email</th>
                  <th>Register At</th>
                  <th>Status</th>
                  <th>Reason</th>
                </tr>
              </thead>
              <tbody v-for="user in users" :key="user.id">
                <tr >
                  <td>{{user.id}}</td>
                  <td>{{user.name}}</td>
                  <td>{{user.email}}</td>
                  <td>{{user.created_at | myDate}}</td>
                  <td><span class="tag tag-success">{{user.type | upText}}</span></td>
                  <td>
                    <a href="">
                      <i class="fas fa-edit text-blue"></i>
                    </a>
                    /
                    <a href="">
                      <i class="fas fa-trash text-red"></i>
                    </a>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
          <!-- /.card-body -->
        </div>
        <!-- /.card -->
      </div>
    </div>
 <!-- Modal -->
<div class="modal fade" id="examplemodal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog modal-dialog-centered">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Add New</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <form @submit.prevent="createUser">
      <div class="modal-body">
        <div class="form-group">
      
      <input v-model="form.name" type="text" name="name" placeholder ="Name"
        class="form-control" :class="{ 'is-invalid': form.errors.has('name') }">
      <has-error :form="form" field="name"></has-error>
    </div>

    <div class="form-group">
      
      <input v-model="form.email" type="email" name="email" placeholder="Email"
        class="form-control" :class="{ 'is-invalid': form.errors.has('email') }">
      <has-error :form="form" field="email"></has-error>
    </div>
    <div class="form-group">
      
      <textarea v-model="form.bio" type="bio" name="bio" placeholder="bio"
        class="form-control" :class="{ 'is-invalid': form.errors.has('bio') }"></textarea>
      <has-error :form="form" field="bio"></has-error>
    </div>
     <div class="form-group">
      
      <select v-model="form.type"  name="type" id="type" 
        class="form-control" :class="{ 'is-invalid': form.errors.has('type') }">
        <option vlaue="">Select User Role</option>
        <option vlaue="admin">Admin</option>
        <option value="author">Author</option>
        <option value= "satndard">Standard</option>
        </select>
      <has-error :form="form" field="type"></has-error>
    </div>
    <div class="form-group">
      
      <input v-model="form.password" type="password" name="password" placeholder="password"
        class="form-control" :class="{ 'is-invalid': form.errors.has('password') }">
      <has-error :form="form" field="password"></has-error>
    </div>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
        <button type="submit" class="btn btn-primary">Save</button>
      </div>
      </form>
    </div>
  </div>
</div>
  </div>
</template>

<script>
export default {
    data(){

        return{
            users: {},
            form: new Form({
                name:'',
                email:'',
                password: '',
                type:'',
                bio:'',
                photo:'',
            })

        }

    },
    methods:{

      loadUsers(){
        axios.get("api/user").then(({data})=>(this.users= data.data));

      },
      createUser(){
       this.$Progress.start();
        this.form.post('api/user');
         $('#examplemodal').modal('hide');
        toast.fire({
         icon: 'success',
           title: 'User created in successfully'
        });
        this.$Progress.finish();
         

        

      },

    },
  created() {
    this.loadUsers();
    //setInterval(()=>this.loadUsers(), 3000);

  },
};
</script>
