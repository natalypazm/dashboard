<template>
  <el-container>
    <el-aside>
      <div class="block pais">
        <el-image :src="src"></el-image>
        <span class="demonstration">PAÍS</span>
        <h5>Perú</h5>
      </div>
      <div>
        <div type="flex" justify="space-around">
          <p>Eduardo Salas</p>
          <small>Administrador País</small>
          <i class="el-icon-s-home"></i>
        </div>
        <div type="flex">
          <i class="el-icon-arrow-left"></i>
          <h1>PUCP</h1>
        </div>
      </div>
      <el-menu
        default-active="3"
        class="el-menu-vertical-demo"
        @open="handleOpen"
        @close="handleClose"
        :collapse="isCollapse"
      >
        <el-menu-item index="1">
          <i class="el-icon-s-home"></i>
          <span slot="title">INICIO</span>
        </el-menu-item>
        <el-menu-item index="2">
          <i class="el-icon-document"></i>
          <span slot="title">REPORTES</span>
        </el-menu-item>
        <el-menu-item index="3">
          <i class="el-icon-tickets"></i>
          <span slot="title">CARRERAS</span>
        </el-menu-item>
        <el-menu-item index="4">
          <i class="el-icon-postcard"></i>
          <span slot="title">CURSOS</span>
        </el-menu-item>
        <el-menu-item index="5">
          <i class="el-icon-s-custom"></i>
          <span slot="title">PROFESORES</span>
        </el-menu-item>
        <el-menu-item index="6">
          <i class="el-icon-user"></i>
          <span slot="title">ALUMNOS</span>
        </el-menu-item>
        <el-menu-item index="7">
          <i class="el-icon-document-copy"></i>
          <span slot="title">CONTENIDO</span>
        </el-menu-item>
        <el-menu-item index="8">
          <i class="el-icon-warning-outline"></i>
          <span slot="title">NO DESEADOS</span>
        </el-menu-item>
        <el-menu-item index="9">
          <i class="el-icon-folder-opened"></i>
          <span slot="title">SOLICITUDES</span>
        </el-menu-item>
      </el-menu>
      <el-radio-group v-model="isCollapse" style="margin-bottom: 20px">
        <el-radio-button :label="true"
          ><i class="el-icon-arrow-left"></i
        ></el-radio-button>
        <el-radio-button :label="false"
          ><i class="el-icon-arrow-right"></i
        ></el-radio-button>
      </el-radio-group>
    </el-aside>
    <el-container style="background: #f9fbfe">
      <el-header class="header">
        <i class="el-icon-arrow-left"></i>
        <span>Nueva Carrera</span>
      </el-header>
      <el-main>
        <el-row>
          <el-col :span="22" :offset="1">
            <el-form :inline="true" class="demo-form-inline">
              <el-row :gutter="20">
                <el-col :span="2"
                  ><el-form-item label="Nombre"></el-form-item>
                </el-col>
                <el-col :span="8">
                  <el-input required v-model="form.name"></el-input>
                </el-col>
                <el-col :span="2"
                  ><el-form-item label="Ciclos"></el-form-item>
                </el-col>
                <el-col :span="5">
                  <el-input-number
                    v-model="form.semestersNumber"
                    @change="handleChange"
                    :min="1"
                    :max="10"
                  ></el-input-number>
                </el-col>
                <el-col :span="6"
                  ><el-form-item>
                    <el-checkbox
                      v-model="form.semesterZero"
                      label="Ciclo 0"
                    ></el-checkbox> </el-form-item
                ></el-col>
                <el-col :span="2">
                  <el-form-item label="Código"></el-form-item>
                </el-col>
                <el-col :span="8">
                  <el-input v-model="form.code" @input="uppercase"></el-input>
                </el-col>
                <el-col :span="2">
                  <el-form-item label="Facultad"></el-form-item>
                </el-col>
                <el-col :span="7">
                  <el-select v-model="form.facultie" placeholder="Seleccione">
                    <el-option
                      v-for="facultie in faculties"
                      :key="facultie.id"
                      :label="facultie.name"
                      :value="facultie.id"
                    >
                    </el-option>
                  </el-select>
                </el-col>
              </el-row>
            </el-form>
            <h1 style="padding;43px 0 20px 0px;">Cursos por Carrera</h1>
            <el-card v-if="form.semesterZero">
              <h5>Ciclo 0</h5>
            </el-card>
            <el-card
              v-for="number in form.semestersNumber"
              :key="number"
              shadow="never"
            >
              <h5>Ciclo {{ number }}</h5>
              <el-select
                v-model="form.semesters[number - 1]"
                @change="onChangedSelected($event)"
                multiple
                filterable
                remote
                reserve-keyword
                placeholder="Agregar curso al ciclo"
              >
                <el-option
                  v-for="course in courses"
                  :key="course.id"
                  :label="course.name"
                  :value="course.id"
                >
                </el-option>
              </el-select>
            </el-card>
          </el-col>
        </el-row>
        <button @click="test">Guardar</button>
      </el-main>
      <el-footer>
        <el-row type="flex" justify="end">
          <el-button round>CANCELAR</el-button>
          <el-button
            @click="onSubmit"
            type="primary"
            round
            :disabled="!validateForm()"
            >GUARDAR</el-button
          >
        </el-row>
      </el-footer>
    </el-container>
  </el-container>
</template>

<script>
import axios from "axios";
import env from "../config/env";
export default {
  data() {
    return {
      form: {
        name: "",
        semestersNumber: 1,
        semesterZero: false,
        code: "",
        facultie: "",
        semesters: [],
      },
      faculties: [],
      courses: [],
      isCollapse: true,
      src:
        "https://fuss10.elemecdn.com/a/3f/3302e58f9a181d2509f3dc0fa68b0jpeg.jpeg",
    };
  },
  created() {
    axios.get(`${env.endpoint}/faculties`).then((response) => {
      this.faculties = response.data;
    });
    axios.get(`${env.endpoint}/courses`).then((response) => {
      this.courses = response.data;
    });
  },
  methods: {
    validateForm() {
      return (
        this.form.name &&
        this.form.code &&
        this.form.facultie &&
        this.validateAlphaNumeric(this.form.name) &&
        this.validateCode(this.form.code)
      );
    },
    validateAlphaNumeric(value) {
      let alphanumeric = /^[a-zA-ZÀ-ÿ0-9\u00f1\u00d1]+(\s*[a-zA-ZÀ-ÿ0-9\u00f1\u00d1]*)*[a-zA-ZÀ-ÿ0-9\u00f1\u00d1]+$/g;
      return alphanumeric.test(value);
    },
    uppercase() {
      console.log("sí");
      this.form.code = this.form.code.toUpperCase();
    },
    validateCode(value) {
      let code = /^[a-z0-9]+$/i;
      return code.test(value);
    },
    onChangedSelected(event) {
      console.log("multiple", event);
    },
    test() {
      console.log(this.form);
    },
    handleOpen(key, keyPath) {
      console.log(key, keyPath);
    },
    handleClose(key, keyPath) {
      console.log(key, keyPath);
    },
    onSubmit() {
      if (this.validateForm()) {
        let payLoad = JSON.parse(JSON.stringify(this.form));
        axios.post(`${env.endpoint}/career`, payLoad).then((response) => {
          //Respuesta del servidor
        });
      }
    },
    handleChange(value) {
      console.log(value);
    },
  },
};
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
aside {
  width: 241px !important;
}
.title {
  font-family: "Open Sans", sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
.pais {
  padding: 50px 20px;
}
.header {
  margin: 60px 38px 6px 0px;
  color: #415061;
  font-size: 36px;
  font-weight: bold;
  letter-spacing: 0.7px;
}
</style>
