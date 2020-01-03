<template>
  <div class="container">
    <div class="row">
      <div class="col-md-12 mt-3">
        <div class="card card-widget widget-user">
          <!-- Add the bg color to the header using any of the bg-* classes -->
          <div class="widget-user-header text-white" style>
            <h3 class="widget-user-username text-right">Elizabeth Pierce</h3>
            <h5 class="widget-user-desc text-right">Web Designer</h5>
          </div>
          <div class="widget-user-image">
            <img class="img-circle" src alt="User Avatar" />
          </div>
          <div class="card-footer">
            <div class="row">
              <div class="col-sm-4 border-right">
                <div class="description-block">
                  <h5 class="description-header">3,200</h5>
                  <span class="description-text">SALES</span>
                </div>
                <!-- /.description-block -->
              </div>
              <!-- /.col -->
              <div class="col-sm-4 border-right">
                <div class="description-block">
                  <h5 class="description-header">13,000</h5>
                  <span class="description-text">FOLLOWERS</span>
                </div>
                <!-- /.description-block -->
              </div>
              <!-- /.col -->
              <div class="col-sm-4">
                <div class="description-block">
                  <h5 class="description-header">35</h5>
                  <span class="description-text">PRODUCTS</span>
                </div>
                <!-- /.description-block -->
              </div>
              <!-- /.col -->
            </div>
            <!-- /.row -->
          </div>
        </div>
      </div>
      <div class="col-md-9">
        <div class="card">
          <div class="card-header p-2">
            <ul class="nav nav-pills">
              <li class="nav-item">
                <a class="nav-link" href="#activity" data-toggle="tab">Activity</a>
              </li>
              <li class="nav-item">
                <a class="nav-link active" href="#settings" data-toggle="tab">Settings</a>
              </li>
            </ul>
          </div>
          <!-- /.card-header -->
          <div class="card-body">
            <div class="tab-content">
              <div class="tab-pane" id="activity">
                <!-- Post -->
                <div class="post">
                  <div class="user-block">
                    <img class="img-circle img-bordered-sm" src alt="user image" />
                    <span class="username">
                      <a href="#">Jonathan Burke Jr.</a>
                      <a href="#" class="float-right btn-tool">
                        <i class="fas fa-times"></i>
                      </a>
                    </span>
                    <span class="description">Shared publicly - 7:30 PM today</span>
                  </div>
                  <!-- /.user-block -->
                  <p>
                    Lorem ipsum represents a long-held
                    tradition for designers, typographers
                    and the like. Some people hate it and
                    argue for its demise, but others ignore
                    the hate as they create awesome tools to
                    help create filler text for everyone
                    from bacon lovers to Charlie Sheen fans.
                  </p>

                  <p>
                    <a href="#" class="link-black text-sm mr-2">
                      <i class="fas fa-share mr-1"></i>
                      Share
                    </a>
                    <a href="#" class="link-black text-sm">
                      <i class="far fa-thumbs-up mr-1"></i>
                      Like
                    </a>
                    <span class="float-right">
                      <a href="#" class="link-black text-sm">
                        <i class="far fa-comments mr-1"></i>Comments (5)
                      </a>
                    </span>
                  </p>

                  <input
                    class="form-control form-control-sm"
                    type="text"
                    placeholder="Type a comment"
                  />
                </div>
                <!-- /.post -->
              </div>

              <div class="tab-pane active" id="settings">
                <form class="form-horizontal">
                  <div class="form-group row">
                    <label for="name" class="col-sm-2 col-form-label">Name</label>
                    <div class="col-sm-10">
                      <input
                        v-model="form.name"
                        type="text"
                        class="form-control"
                        id="name"
                        placeholder="Name"
                        :class="{'is-invalid': form.errors.has('name')}"
                      />
                      <has-error :form="form" field="name"></has-error>
                    </div>
                  </div>
                  <div class="form-group row">
                    <label for="email" class="col-sm-2 col-form-label">Email</label>
                    <div class="col-sm-10">
                      <input
                        v-model="form.email"
                        type="email"
                        class="form-control"
                        id="email"
                        placeholder="Email Address"
                        :class="{'is-invalid': form.errors.has('name')}"
                      />
                      <has-error :form="form" field="email"></has-error>
                    </div>
                  </div>
                  <div class="form-group row">
                    <label for="experience" class="col-sm-2 col-form-label">Experience</label>
                    <div class="col-sm-10">
                      <textarea
                        type="text"
                        class="form-control"
                        id="experience"
                        placeholder="Experience"
                      ></textarea>
                    </div>
                  </div>
                  <div class="form-group row">
                    <label for="photo" class="col-sm-2 col-form-label">Photo Profile</label>
                    <div
                      class="input-group col-sm-10"
                      :class="{'is-invalid': form.errors.has('name')}"
                    >
                      <div class="custom-file">
                        <input
                          type="file"
                          class="custom-file-input"
                          id="photo"
                          accept=".jpg, .png, .jpeg"
                          @change="updatePhoto"
                        />
                        <label class="custom-file-label" for="photo">Choose file</label>
                      </div>
                    </div>
                    <has-error :form="form" field="photo"></has-error>
                  </div>
                  <div class="form-group row">
                    <label for="passsport" class="col-sm-2 col-form-label">Passport</label>
                    <div class="col-sm-10">
                      <input
                        type="text"
                        class="form-control"
                        id="passport"
                        placeholder="Passport (leave empty if not changing)"
                      />
                    </div>
                  </div>
                  <div class="form-group row">
                    <div class="offset-sm-2 col-sm-10">
                      <button type="submit" class="btn btn-success">Update</button>
                    </div>
                  </div>
                </form>
              </div>
              <!-- /.tab-pane -->
            </div>
            <!-- /.tab-content -->
          </div>
          <!-- /.card-body -->
        </div>
        <!-- /.nav-tabs-custom -->
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: new Form({
        id: "",
        name: "",
        email: "",
        password: "",
        type: "",
        bio: "",
        photo: ""
      })
    };
  },

  methods: {
    updatePhoto(e) {
      let file = e.target.files[0];
      let reader = new FileReader();

      reader.onloadend = (file) => {
        console.log(reader);
        this.form.photo = reader.result;
      }
    }
  },

  mounted() {
    console.log("Component mounted.");
    bsCustomFileInput.init();
    axios.get("api/profile").then(({ data }) => this.form.fill(data));
  }
};
</script>
