<template>
  <el-container>
    <el-aside>
      <div class="pais">
        <img src="~/assets/images/logowabu.png" alt="logo" />
        <div class="info-city">
          <span>PAÍS</span>
          <h5>Perú</h5>
        </div>
      </div>
      <div>
        <div class="box-user">
          <div>
            <p class="user-name">Eduardo Salas</p>
            <small>Administrador País</small>
          </div>
          <img src="~/assets/images/icon_logout.png" alt="logout" />
        </div>
        <div class="name-college">
          <img src="~/assets/images/arrow.svg" alt="arrow-left" />
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
        <img src="~/assets/images/arrow.svg" alt="arrow-left" />
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
            <h1 class="title" style="padding: 43px 0 20px 0px">
              Cursos por Carrera
            </h1>
            <el-card v-if="form.semesterZero" shadow="never">
              <h5 class="number-semester">Ciclo 0</h5>
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
            <el-card
              v-for="number in form.semestersNumber"
              :key="number"
              shadow="never"
            >
              <h5 class="number-semester">Ciclo {{ number }}</h5>
              <!-- <el-select
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
              </el-select> -->
              <el-tag
                :key="tag"
                v-for="tag in dynamicTags"
                closable
                :disable-transitions="false"
                @close="handleClose(tag)"
              >
                {{ tag }}
              </el-tag>
              <div v-if="inputVisible">
                <el-input
                  class="input-new-tag"
                  v-model="inputValue"
                  ref="saveTagInput"
                  size="mini"
                  @keyup.enter.native="handleInputConfirm"
                  @blur="handleInputConfirm"
                  v-for="course in courses"
                  :key="course.id"
                  :label="course.name"
                  :value="course.id"
                >
                </el-input>
              </div>
              <el-button
                v-else
                class="button-new-tag"
                size="small"
                @click="showInput"
                >+ New Tag</el-button
              >
            </el-card>
          </el-col>
        </el-row>
        <!-- <button @click="test">Guardar</button> -->
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
      dynamicTags: [],
      inputVisible: false,
      inputValue: "",
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
      isCollapse: false,
      src: "assests/images/logowabu.png",
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
    handleClose(tag) {
      this.dynamicTags.splice(this.dynamicTags.indexOf(tag), 1);
    },

    showInput() {
      this.inputVisible = true;
      this.$nextTick((_) => {
        console.log("hola");
        // this.$refs.saveTagInput.$refs.input.focus();
      });
    },

    handleInputConfirm() {
      let inputValue = this.inputValue;
      if (inputValue) {
        this.dynamicTags.push(inputValue);
      }
      this.inputVisible = false;
      this.inputValue = "";
    },
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
body {
  font-family: "Open Sans", sans-serif;
}
.el-input {
  width: auto;
}
.el-input__inner:focus {
  border-color: #1283ff;
}
.el-form-item__label,
.el-input__inner {
  color: #909399;
}
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
  background-color: #fcfdff;
  box-shadow: 2px 6px 12px 0 rgba(198, 212, 212, 0.33);
}
ul.el-menu-vertical-demo.el-menu {
  margin-top: 28px;
  background: transparent;
  border: none;
}
li.el-menu-item {
  color: #415061;
  font-size: 12px;
  font-weight: bold;
  letter-spacing: 0.72px;
  height: 40px;
  line-height: 40px;
  margin-bottom: 10px;
}
.el-menu-item.is-active {
  background-color: rgba(18, 131, 255, 0.05);
  box-shadow: 0 1px 1px 0 rgba(0, 0, 0, 0.03);
  border-left: 4px solid #1283ff;
}
.title {
  color: #1283ff;
  font-size: 20px;
  font-weight: 600;
  letter-spacing: 0.4px;
  line-height: 23px;
}
.number-semester {
  color: #415061;
  font-size: 16px;
  font-weight: 600;
  letter-spacing: 0.32px;
  line-height: 18px;
  margin-bottom: 12px;
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
  padding: 35px 20px 45px 20px;
  display: flex;
  align-items: flex-end;
}
.pais img {
  margin-bottom: 10px;
}
.pais .info-city {
  border-left: 1px solid #7b8fa0;
  padding-left: 13px;
  margin-left: 13px;
  padding-top: 10px;
}
.pais .info-city span {
  color: #7b8fa0;
  font-size: 10px;
  letter-spacing: 0.2px;
  line-height: 14px;
}
.pais .info-city h5 {
  color: #415061;
  font-size: 18px;
  font-weight: bold;
  letter-spacing: 0.36px;
  line-height: 21px;
}
.box-user {
  padding: 0px 20px;
  display: flex;
  justify-content: space-between;
}
.box-user .user-name {
  color: #415061;
  font-size: 17px;
  font-weight: 600;
  letter-spacing: 0.34px;
  line-height: 10px;
}
.box-user small {
  color: #7b8fa0;
  font-size: 11px;
  letter-spacing: 0.22px;
}
.box-user img {
  height: 25px;
  cursor: pointer;
}
.name-college {
  padding: 0px 20px;
  display: flex;
  margin-top: 5px;
}
.name-college h1 {
  color: #1283ff;
  font-size: 20px;
  font-weight: bold;
  letter-spacing: 0.4px;
  line-height: 23px;
  margin-left: 10px;
}

.header {
  margin: 60px 38px 6px 0px;
  color: #415061;
  font-size: 36px;
  font-weight: bold;
  letter-spacing: 0.7px;
  display: flex;
  align-items: center;
}
.header i {
  font-size: 15px;
  font-weight: bold;
  margin-right: 16px;
}
/* AGREGAR CURSO */
.el-select .el-select__tags > span {
  display: flex;
}
.el-select {
  width: 100%;
}
.el-tag.el-tag--info {
  background-color: #1283ff;
  border-color: #1283ff;
  color: #fff;
}
.el-tag.el-tag--info .el-tag__close {
  font-size: 15px;
  font-weight: bold;
  color: #fff;
  background: transparent;
}
</style>
