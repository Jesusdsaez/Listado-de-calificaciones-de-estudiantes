<script setup>
import { reactive } from "vue";

const alumnos = reactive([])
const asignaturas = reactive([])
const notas = reactive([])

const alumno = reactive({
  cedula: "",
  nombre: "",
  apellido: "",
  edad: 0,
  correo: ""
})

const asignatura = reactive({
  nombre: "",
  codigo: ""
})

const nota = reactive({
  cedula_estudiante: "",
  codigo_asignatura: "",
  nota1: 0,
  nota2: 0,
  nota3: 0,
  id: null
})

// FUNCIONES ALUMNOS
function registraralumno(){
  if(!alumno.cedula || !alumno.nombre || !alumno.apellido){
    alert("Por favor completa los campos requeridos");
    return;
  }
  alumnos.push({...alumno});
  alert("Alumno registrado correctamente");
  limpiar();
  const modal = document.getElementById('modalalumno');
  if(modal) {
    bootstrap.Modal.getInstance(modal)?.hide();
  }
}

// FUNCIONES ASIGNATURAS
function registrasignatura(){
  if(!asignatura.nombre || !asignatura.codigo){
    alert("Por favor completa todos los campos");
    return;
  }
  asignaturas.push({...asignatura});
  alert("Asignatura registrada correctamente");
  limpiar();
  const modal = document.getElementById('modalasignatura');
  if(modal) {
    bootstrap.Modal.getInstance(modal)?.hide();
  }
}

// FUNCIONES CALIFICACIONES / NOTAS
function registrarNota(){
  if(!nota.cedula_estudiante || !nota.codigo_asignatura || !nota.nota1 || !nota.nota2 || !nota.nota3){
    alert("Por favor completa todos los campos");
    return;
  }
  
  if(nota.id !== null){
    const index = notas.findIndex(n => n.id === nota.id);
    if(index !== -1){
      notas[index] = {
        cedula_estudiante: nota.cedula_estudiante,
        codigo_asignatura: nota.codigo_asignatura,
        nota1: Number(nota.nota1),
        nota2: Number(nota.nota2),
        nota3: Number(nota.nota3),
        id: nota.id
      };
    }
    alert("Nota actualizada correctamente");
  } else {
    notas.push({
      cedula_estudiante: nota.cedula_estudiante,
      codigo_asignatura: nota.codigo_asignatura,
      nota1: Number(nota.nota1),
      nota2: Number(nota.nota2),
      nota3: Number(nota.nota3),
      id: Date.now()
    });
    alert("Nota registrada correctamente");
  }
  limpiarNota();
  const modal = document.getElementById('modalnota');
  if(modal) {
    bootstrap.Modal.getInstance(modal)?.hide();
  }
}

function limpiarNota(){
  nota.cedula_estudiante = "";
  nota.codigo_asignatura = "";
  nota.nota1 = 0;
  nota.nota2 = 0;
  nota.nota3 = 0;
  nota.id = null;
}

function editarNota(notaItem){
  nota.cedula_estudiante = notaItem.cedula_estudiante;
  nota.codigo_asignatura = notaItem.codigo_asignatura;
  nota.nota1 = notaItem.nota1;
  nota.nota2 = notaItem.nota2;
  nota.nota3 = notaItem.nota3;
  nota.id = notaItem.id;
  const modal = new bootstrap.Modal(document.getElementById('modalnota'));
  modal.show();
}

function eliminarNota(id){
  if(confirm("¿Estás seguro de que quieres eliminar esta nota?")){
    const index = notas.findIndex(n => n.id === id);
    if(index !== -1){
      notas.splice(index, 1);
      alert("Nota eliminada correctamente");
    }
  }
}

function calcularPromedio(nota1, nota2, nota3){
  return ((parseFloat(nota1) + parseFloat(nota2) + parseFloat(nota3)) / 3).toFixed(2);
}

function limpiar(){
  alumno.cedula = "";
  alumno.nombre = "";
  alumno.apellido = "";
  alumno.edad = 0;
  alumno.correo = "";
  asignatura.nombre = "";
  asignatura.codigo = "";
  limpiarNota();
}
</script>

<template>
  <div>
    <!-- Barra de Navegación Simple -->
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
      <div class="container-fluid">
        <a class="navbar-brand fw-bold" href="#">SISTEMA DE CALIFICACIONES</a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <!-- Menú Alumnos -->
            <li class="nav-item dropdown">
              <a class="nav-link dropdown-toggle text-success fw-semibold" href="#" role="button" data-bs-toggle="dropdown">
                Alumnos
              </a>
              <ul class="dropdown-menu">
                <li><a class="dropdown-item" data-bs-toggle="modal" data-bs-target="#modalalumno" href="#">Registrar</a></li>
                <li><a class="dropdown-item" data-bs-toggle="modal" data-bs-target="#modallistaralumno" href="#">Listar</a></li>
              </ul>
            </li>

            <!-- Menú Asignaturas -->
            <li class="nav-item dropdown">
              <a class="nav-link dropdown-toggle text-warning fw-semibold" href="#" role="button" data-bs-toggle="dropdown">
                Asignaturas
              </a>
              <ul class="dropdown-menu">
                <li><a class="dropdown-item" data-bs-toggle="modal" data-bs-target="#modalasignatura" href="#">Registrar</a></li>
                <li><a class="dropdown-item" data-bs-toggle="modal" data-bs-target="#modallistarasignatura" href="#">Listar</a></li>
              </ul>
            </li>

            <!-- Menú Calificaciones -->
            <li class="nav-item dropdown">
              <a class="nav-link dropdown-toggle text-info fw-semibold" href="#" role="button" data-bs-toggle="dropdown">
                Calificaciones
              </a>
              <ul class="dropdown-menu">
                <li><a class="dropdown-item" data-bs-toggle="modal" data-bs-target="#modalnota" href="#">Registrar</a></li>
                <li><a class="dropdown-item" data-bs-toggle="modal" data-bs-target="#modallistarnotas" href="#">Listar</a></li>
              </ul>
            </li>        
          </ul>
        </div>
      </div>
    </nav>

    <!-- Modal Registro Alumno (Cuadrito Verde Suave) -->
    <div class="modal fade" id="modalalumno" tabindex="-1">
      <div class="modal-dialog">
        <div class="modal-content border border-success-subtle shadow-sm" style="background-color: #f0fdf4;">
          <div class="modal-header text-white" style="background-color: #198754;">
            <h5 class="modal-title fw-bold">Registro de Alumnos</h5>
            <button type="button" class="btn-close btn-close-white" data-bs-dismiss="modal"></button>
          </div>
          <div class="modal-body">
            <div class="form-floating mb-3">
              <input type="number" class="form-control border-success-subtle" id="cedulaAlumno" placeholder="Cedula" v-model="alumno.cedula">
              <label for="cedulaAlumno">Cédula</label>
            </div>
            <div class="form-floating mb-3">
              <input type="text" class="form-control border-success-subtle" id="nombreAlumno" placeholder="Nombre" v-model="alumno.nombre">
              <label for="nombreAlumno">Nombre</label>
            </div>
            <div class="form-floating mb-3">
              <input type="text" class="form-control border-success-subtle" id="apellidoAlumno" placeholder="Apellido" v-model="alumno.apellido">
              <label for="apellidoAlumno">Apellido</label>
            </div>
            <div class="form-floating mb-3">
              <input type="number" class="form-control border-success-subtle" id="edadAlumno" placeholder="Edad" v-model="alumno.edad">
              <label for="edadAlumno">Edad</label>
            </div>
            <div class="form-floating mb-3">
              <input type="email" class="form-control border-success-subtle" id="correoAlumno" placeholder="Correo" v-model="alumno.correo">
              <label for="correoAlumno">Correo</label>
            </div>
          </div>
          <div class="modal-footer border-top border-success-subtle" style="background-color: #e8f7ee;">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
            <button type="button" class="btn btn-success" @click="registraralumno">Guardar Alumno</button>
          </div>
        </div>
      </div>
    </div>

    <!-- Modal Registro Asignatura (Cuadrito Amarillo/Ámbar Suave) -->
    <div class="modal fade" id="modalasignatura" tabindex="-1">
      <div class="modal-dialog">
        <div class="modal-content border border-warning-subtle shadow-sm" style="background-color: #fefce8;">
          <div class="modal-header text-dark" style="background-color: #facc15;">
            <h5 class="modal-title fw-bold">Registro de Asignaturas</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
          </div>
          <div class="modal-body">
            <div class="form-floating mb-3">
              <input type="text" class="form-control border-warning-subtle" id="nombreAsignatura" placeholder="Nombre" v-model="asignatura.nombre">
              <label for="nombreAsignatura">Nombre</label>
            </div>
            <div class="form-floating mb-3">
              <input type="text" class="form-control border-warning-subtle" id="codigoAsignatura" placeholder="Código" v-model="asignatura.codigo">
              <label for="codigoAsignatura">Código</label>
            </div>
          </div>
          <div class="modal-footer border-top border-warning-subtle" style="background-color: #fef9c3;">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
            <button type="button" class="btn btn-warning text-dark fw-bold" @click="registrasignatura">Guardar Asignatura</button>
          </div>
        </div>
      </div>
    </div>

    <!-- Modal Registro de Notas (Cuadrito Azul Suave) -->
    <div class="modal fade" id="modalnota" tabindex="-1">
      <div class="modal-dialog">
        <div class="modal-content border border-primary-subtle shadow-sm" style="background-color: #eff6ff;">
          <div class="modal-header text-white" style="background-color: #2563eb;">
            <h5 class="modal-title fw-bold">Registro de Notas</h5>
            <button type="button" class="btn-close btn-close-white" data-bs-dismiss="modal"></button>
          </div>
          <div class="modal-body">
            <div class="form-floating mb-3">
              <select class="form-select border-primary-subtle" id="selectAlumno" v-model="nota.cedula_estudiante">
                <option value="">Selecciona un estudiante</option>
                <option v-for="al in alumnos" :key="al.cedula" :value="al.cedula">
                  {{ al.cedula }} - {{ al.nombre }} {{ al.apellido }}
                </option>
              </select>
              <label for="selectAlumno">Estudiante</label>
            </div>
            <div class="form-floating mb-3">
              <select class="form-select border-primary-subtle" id="selectAsignatura" v-model="nota.codigo_asignatura">
                <option value="">Selecciona una asignatura</option>
                <option v-for="asig in asignaturas" :key="asig.codigo" :value="asig.codigo">
                  {{ asig.codigo }} - {{ asig.nombre }}
                </option>
              </select>
              <label for="selectAsignatura">Asignatura</label>
            </div>
            <div class="form-floating mb-3">
              <input type="number" class="form-control border-primary-subtle" id="nota1" placeholder="Nota 1" v-model.number="nota.nota1" min="1" max="5" step="0.1">
              <label for="nota1">Nota 1 (1-5)</label>
            </div>
            <div class="form-floating mb-3">
              <input type="number" class="form-control border-primary-subtle" id="nota2" placeholder="Nota 2" v-model.number="nota.nota2" min="1" max="5" step="0.1">
              <label for="nota2">Nota 2 (1-5)</label>
            </div>
            <div class="form-floating mb-3">
              <input type="number" class="form-control border-primary-subtle" id="nota3" placeholder="Nota 3" v-model.number="nota.nota3" min="1" max="5" step="0.1">
              <label for="nota3">Nota 3 (1-5)</label>
            </div>
          </div>
          <div class="modal-footer border-top border-primary-subtle" style="background-color: #dbeafe;">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
            <button type="button" class="btn btn-primary" @click="registrarNota">Guardar Nota</button>
          </div>
        </div>
      </div>
    </div>

    <!-- Listar Alumnos (Cuadro Verde) -->
    <div class="modal fade" id="modallistaralumno" tabindex="-1">
      <div class="modal-dialog modal-lg">
        <div class="modal-content border border-success-subtle shadow-sm" style="background-color: #fbfdfb;">
          <div class="modal-header text-white" style="background-color: #198754;">
            <h5 class="modal-title fw-bold">Listado de Alumnos</h5>
            <button type="button" class="btn-close btn-close-white" data-bs-dismiss="modal"></button>
          </div>
          <div class="modal-body p-0">
            <div v-if="alumnos.length === 0" class="alert alert-success m-3">
              No hay alumnos registrados
            </div>
            <table v-else class="table table-hover table-striped mb-0">
              <thead class="table-success">
                <tr>
                  <th>Cédula</th>
                  <th>Nombre</th>
                  <th>Apellido</th>
                  <th>Edad</th>
                  <th>Correo</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="data in alumnos" :key="data.cedula">
                  <td class="fw-semibold">{{ data.cedula }}</td>
                  <td>{{ data.nombre }}</td>
                  <td>{{ data.apellido }}</td>
                  <td>{{ data.edad }}</td>
                  <td>{{ data.correo }}</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="modal-footer border-top border-success-subtle" style="background-color: #f0fdf4;">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
          </div>
        </div>
      </div>
    </div>

    <!-- Listar Asignaturas (Cuadro Ámbar) -->
    <div class="modal fade" id="modallistarasignatura" tabindex="-1">
      <div class="modal-dialog modal-lg">
        <div class="modal-content border border-warning-subtle shadow-sm" style="background-color: #fffef7;">
          <div class="modal-header text-dark" style="background-color: #facc15;">
            <h5 class="modal-title fw-bold">Listado de Asignaturas</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
          </div>
          <div class="modal-body p-0">
            <div v-if="asignaturas.length === 0" class="alert alert-warning m-3">
              No hay asignaturas registradas
            </div>
            <table v-else class="table table-hover table-striped mb-0">
              <thead class="table-warning">
                <tr>
                  <th>Nombre</th>
                  <th>Código</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="data in asignaturas" :key="data.codigo">
                  <td class="fw-semibold">{{ data.nombre }}</td>
                  <td><span class="badge bg-dark">{{ data.codigo }}</span></td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="modal-footer border-top border-warning-subtle" style="background-color: #fefce8;">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
          </div>
        </div>
      </div>
    </div>

    <!-- Listar Calificaciones (Cuadro Azul) -->
    <div class="modal fade" id="modallistarnotas" tabindex="-1">
      <div class="modal-dialog modal-xl">
        <div class="modal-content border border-primary-subtle shadow-sm" style="background-color: #f8faff;">
          <div class="modal-header text-white" style="background-color: #2563eb;">
            <h5 class="modal-title fw-bold">Listado de Calificaciones</h5>
            <button type="button" class="btn-close btn-close-white" data-bs-dismiss="modal"></button>
          </div>
          <div class="modal-body p-0">
            <div v-if="notas.length === 0" class="alert alert-primary m-3">
              No hay calificaciones registradas
            </div>
            <table v-else class="table table-hover table-striped mb-0">
              <thead class="table-primary">
                <tr>
                  <th>Cédula Estudiante</th>
                  <th>Código Asignatura</th>
                  <th class="text-center">Nota 1</th>
                  <th class="text-center">Nota 2</th>
                  <th class="text-center">Nota 3</th>
                  <th class="text-center">Promedio</th>
                  <th class="text-center">Acciones</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="notaItem in notas" :key="notaItem.id">
                  <td>{{ notaItem.cedula_estudiante }}</td>
                  <td><span class="badge bg-secondary">{{ notaItem.codigo_asignatura }}</span></td>
                  <td class="text-center">{{ notaItem.nota1 }}</td>
                  <td class="text-center">{{ notaItem.nota2 }}</td>
                  <td class="text-center">{{ notaItem.nota3 }}</td>
                  <td class="text-center">
                    <span class="badge" :class="calcularPromedio(notaItem.nota1, notaItem.nota2, notaItem.nota3) >= 3.0 ? 'bg-success' : 'bg-danger'">
                      {{ calcularPromedio(notaItem.nota1, notaItem.nota2, notaItem.nota3) }}
                    </span>
                  </td>
                  <td class="text-center">
                    <button class="btn btn-sm btn-outline-warning me-1" @click="editarNota(notaItem)">Editar</button>
                    <button class="btn btn-sm btn-outline-danger" @click="eliminarNota(notaItem.id)">Eliminar</button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="modal-footer border-top border-primary-subtle" style="background-color: #eff6ff;">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>