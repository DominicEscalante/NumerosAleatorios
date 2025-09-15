<template>
  <div class="lineal-container">
    <Navbar />
    <Background />

    <div class="content-wrapper">
      <h1 class="main-title">Algoritmo Congruencial Multiplicativa</h1>

      <div class="algorithm-description">
        <p>
          Fórmula: <span class="formula">X<sub>i</sub> = (a × X<sub>i-1</sub>) mod (m)</span>
        </p>
      </div>

      <form @submit.prevent="calcular" class="space-form">
        <div class="inputs-grid">
          <div class="input-group">
            <label class="input-label">
              <span class="label-icon">🎯</span>
              X₀ (Semilla inicial)
            </label>
            <input v-model="x0" type="number" class="space-input" placeholder="Ej: 5" required />
          </div>

          <div class="input-group">
            <label class="input-label">
              <span class="label-icon">⚡</span>
              K
            </label>
            <input v-model="k" type="number" class="space-input" placeholder="Ej: 3" required />
          </div>

          <div class="input-group">
            <label class="input-label">
              <span class="label-icon">➕</span>
              a (Constante multiplicativa)
            </label>

            <div class="radio-group">
              <label class="radio-option">
                <input type="radio" v-model="a" value="1" class="radio-input" />
                <span class="custom-radio"></span>
                3 + 8K
              </label>

              <label class="radio-option">
                <input type="radio" v-model="a" value="2" class="radio-input" />
                <span class="custom-radio"></span>
                5 + 8K
              </label>
            </div>
          </div>

          <div class="input-group">
            <label class="input-label">
              <span class="label-icon">🔢</span>
              P (Periodo)
            </label>
            <input v-model="p" type="number" class="space-input" placeholder="Ej: 11" required />
          </div>

          <div class="input-group">
            <label class="input-label">
              <span class="label-icon">🔢</span>
              D (Cantidad de decimales)
            </label>
            <input v-model="d" type="number" class="space-input" placeholder="Ej: 2" required />
          </div>
        </div>

        <button type="submit" class="generate-btn">
          <span class="btn-icon">🚀</span>
          Generar Secuencia
        </button>
      </form>

      <div class="parameters-display" v-if="parametros.a">
        <h2 class="parameters-title">Parámetros Utilizados</h2>
        <ul class="parameters-list">
          <li><strong>A:</strong> {{ parametros.aCalculada }}</li>
          <li><strong>G:</strong> {{ parametros.g }}</li>
          <li><strong>M:</strong> {{ parametros.m }}</li>
        </ul>
      </div>

      <div v-if="resultado.length" class="results-container">
        <div class="results-header">
          <h2 class="results-title">
            <span class="results-icon">📊</span>
            Secuencia Generada
          </h2>
          <div class="results-stats">
            Total: <span class="stat-number">{{ resultado.length }}</span>
          </div>
        </div>

        <div class="results-list">
          <div v-for="(item, index) in resultado" :key="index" class="result-item">
            <div class="result-step">{{ index + 1 }}</div>
            <div class="result-formula">
              <span>
                ({{ parametros.aCalculada }} × {{ item.xAnterior }}) mod {{ item.p }} =
                {{ item.x }}
              </span>
            </div>
            <div class="result-value">{{ item.valor }}</div>
          </div>
        </div>

        <button @click="limpiar" class="clear-btn">🗑️ Limpiar Resultados</button>
      </div>
    </div>
  </div>
</template>

<script>
import Background from '../components/Background.vue'
import Navbar from '@/components/Navbar.vue'

export default {
  name: 'LinealView',
  components: {
    Background,
    Navbar,
  },
  data() {
    return {
      x0: '',
      k: '',
      a: '',
      p: '',
      d: '',
      resultado: [],
      parametros: {
        g: '',
        m: '',
        aCalculada: '',
      },
    }
  },
  methods: {
    calcular() {
      this.resultado = []

      let x = parseInt(this.x0)
      const kVal = parseInt(this.k)
      const cVal = parseInt(this.c)
      const pVal = parseInt(this.p)
      const numIter = parseInt(this.p)
      const decimales = parseInt(this.d)
      const aOption = parseInt(this.a)

      if (x < 0 || kVal < 0 || cVal < 0 || pVal <= 0 || decimales < 0) {
        alert(
          'Por favor, ingrese valores numéricos válidos y no negativos. Todos los parametros deben ser mayores que 0.',
        )
        this.parametros = {
          a: '',
          g: '',
          m: '',
        }
        return
      }

      if (x % 2 === 0) {
        alert('X0 no puede ser un numero par, introduzca un numero impar.')
        this.parametros = {
          a: '',
          g: '',
          m: '',
        }
        return
      }

      if (aOption !== 1 && aOption !== 2) {
        alert('Por favor, seleccione una opción válida para "a".')
        this.parametros = {
          a: '',
          g: '',
          m: '',
        }
        return
      }

      let a = 0

      if (aOption === 1) {
        a = 3 + 8 * kVal
        this.parametros.aCalculada = a
      } else if (aOption === 2) {
        a = 5 + 8 * kVal
        this.parametros.aCalculada = a
      }

      const g = Math.log(pVal) / Math.log(2) + 2
      const m = Math.pow(2, g)

      this.parametros.a = a
      this.parametros.g = Math.ceil(g)
      this.parametros.m = m
      this.parametros.c = cVal

      // Generar secuencia
      for (let i = 0; i < numIter; i++) {
        const xAnterior = x
        x = (a * x) % m
        const r = x / (m - 1)

        this.resultado.push({
          x: x,
          a: a,
          valor: r.toFixed(decimales),
          k: kVal,
          c: cVal,
          p: pVal,
          xAnterior: xAnterior,
        })
      }
    },

    limpiar() {
      this.parametros = {
        a: '',
        g: '',
        m: '',
        c: '',
      }
      this.resultado = []
      this.x0 = ''
      this.k = ''
      this.aCalculada = ''
      this.p = ''
      this.d = ''
    },
  },
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;500;700;900&display=swap');

.lineal-container {
  margin: 0;
  padding: 0;
  width: 100%;
  min-height: 100vh;
  position: relative;
  font-family: 'Orbitron', sans-serif;
}

.content-wrapper {
  position: relative;
  z-index: 10;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 2rem 1rem;
  gap: 2rem;
}

/* Título principal */
.main-title {
  font-size: 3rem;
  font-weight: 900;
  background: linear-gradient(90deg, #00f2fe, #4facfe, #00c9ff);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-shadow: 0 0 30px rgba(0, 200, 255, 0.5);
  text-align: center;
  margin-bottom: 0.5rem;
}

/* Descripción del algoritmo */
.algorithm-description {
  text-align: center;
  color: #cceeff;
  margin-bottom: 1rem;
}

.formula {
  color: #00f2fe;
  font-weight: 700;
  background: rgba(0, 242, 254, 0.15);
  padding: 0.5rem 1rem;
  border-radius: 0.5rem;
  border: 1px solid rgba(0, 242, 254, 0.4);
}

.parameters-title {
  text-align: center;
  color: #cceeff;
  margin-bottom: 1rem;
}

.parameters-list {
  color: #00f2fe;
  font-weight: 700;
  background: rgba(0, 242, 254, 0.15);
  padding: 0.5rem 1rem;
  border-radius: 0.5rem;
  border: 1px solid rgba(0, 242, 254, 0.4);
  list-style: none;
  text-align: center;
  column-count: 2;
  column-gap: 1rem;
}

.space-form {
  background: radial-gradient(circle at center, rgba(40, 60, 100, 0.9), rgba(10, 10, 30, 0.95));
  backdrop-filter: blur(20px);
  border: 2px solid rgba(120, 200, 255, 0.5);
  border-radius: 2rem;
  padding: 3rem;
  width: 100%;
  max-width: 600px;
  box-shadow:
    0 0 40px rgba(0, 150, 255, 0.3),
    inset 0 0 20px rgba(0, 100, 200, 0.1);
}

.inputs-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 2rem;
  margin-bottom: 2rem;
}

.input-group {
  display: flex;
  flex-direction: column;
}

.input-group.full-width {
  grid-column: 1 / -1;
}

.input-label {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 0.8rem;
  color: #a0d0ff;
  font-weight: 600;
  font-size: 0.95rem;
}

.label-icon {
  font-size: 1.2rem;
}

.space-input {
  padding: 1rem 1.5rem;
  background: rgba(0, 0, 0, 0.6);
  border: 2px solid rgba(120, 200, 255, 0.4);
  border-radius: 1rem;
  color: #ffffff;
  font-size: 1rem;
  font-family: 'Orbitron', sans-serif;
  font-weight: 500;
  transition: all 0.3s ease;
  outline: none;
}

.space-input:focus {
  border-color: #00d4ff;
  box-shadow: 0 0 20px rgba(0, 212, 255, 0.4);
  background: rgba(0, 20, 40, 0.8);
}

.radio-group {
  display: flex;
  gap: 1.5rem;
  flex-direction: column;
  align-items: center;
}

/* Ocultamos el input real */
.radio-input {
  display: none;
}

/* Label del radio */
.radio-option {
  display: flex;
  align-items: center;
  gap: 0.6rem;
  cursor: pointer;
  font-family: 'Orbitron', sans-serif;
  font-size: 1rem;
  font-weight: 500;
  color: #ffffff;
  transition: color 0.3s ease;
}

/* Estilo del círculo */
.custom-radio {
  width: 20px;
  height: 20px;
  border: 2px solid rgba(120, 200, 255, 0.6);
  border-radius: 50%;
  background: rgba(0, 0, 0, 0.6);
  box-shadow: 0 0 10px rgba(0, 212, 255, 0.2);
  transition: all 0.3s ease;
}

/* Cuando está seleccionado */
.radio-input:checked + .custom-radio {
  background: #00d4ff;
  box-shadow: 0 0 20px rgba(0, 212, 255, 0.6);
  border-color: #00d4ff;
}

/* Hover */
.radio-option:hover .custom-radio {
  border-color: #00d4ff;
  box-shadow: 0 0 15px rgba(0, 212, 255, 0.5);
}

.space-input::placeholder {
  color: #6a9bd1;
  opacity: 0.7;
}

/* Botón generar */
.generate-btn {
  width: 100%;
  padding: 1.2rem 2rem;
  background: linear-gradient(135deg, #00d4ff, #4facfe);
  border: none;
  border-radius: 1.2rem;
  color: white;
  font-family: 'Orbitron', sans-serif;
  font-weight: 700;
  font-size: 1.1rem;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.8rem;
  margin-top: 1rem;
  box-shadow: 0 4px 20px rgba(0, 212, 255, 0.3);
}

.generate-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 30px rgba(0, 212, 255, 0.5);
}

.btn-icon {
  font-size: 1.2rem;
}

/* Contenedor de resultados */
.results-container {
  width: 100%;
  max-width: 800px;
  background: radial-gradient(circle at top, rgba(40, 60, 100, 0.9), rgba(10, 10, 30, 0.95));
  backdrop-filter: blur(15px);
  border: 2px solid rgba(120, 200, 255, 0.5);
  border-radius: 2rem;
  padding: 2rem;
  box-shadow: 0 0 40px rgba(0, 150, 255, 0.2);
}

.results-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2rem;
  flex-wrap: wrap;
  gap: 1rem;
}

.results-title {
  display: flex;
  align-items: center;
  gap: 0.8rem;
  color: #00f2fe;
  font-size: 1.8rem;
  font-weight: 700;
}

.results-icon {
  font-size: 1.5rem;
}

.results-stats {
  background: rgba(0, 212, 255, 0.15);
  padding: 0.5rem 1rem;
  border-radius: 0.8rem;
  border: 1px solid rgba(0, 212, 255, 0.4);
  color: #a0d0ff;
}

.stat-number {
  color: #00f2fe;
  font-weight: 700;
}

/* Lista de resultados */
.results-list {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  max-height: 400px;
  overflow-y: auto;
  padding-right: 0.5rem;
}

.results-list::-webkit-scrollbar {
  width: 6px;
}

.results-list::-webkit-scrollbar-track {
  background: rgba(0, 0, 0, 0.3);
  border-radius: 3px;
}

.results-list::-webkit-scrollbar-thumb {
  background: linear-gradient(180deg, #00d4ff, #4facfe);
  border-radius: 3px;
}

.result-item {
  display: grid;
  grid-template-columns: auto 1fr auto;
  gap: 1rem;
  align-items: center;
  background: rgba(0, 20, 40, 0.7);
  padding: 1rem;
  border-radius: 1rem;
  border: 1px solid rgba(0, 212, 255, 0.3);
}

.result-step {
  background: linear-gradient(135deg, #00d4ff, #4facfe);
  color: white;
  font-weight: 700;
  padding: 0.5rem;
  border-radius: 50%;
  min-width: 2.5rem;
  height: 2.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.9rem;
}

.result-formula {
  color: #cceeff;
  font-family: 'Courier New', monospace;
  font-size: 0.9rem;
  flex: 1;
}

.result-value {
  background: rgba(0, 242, 254, 0.15);
  color: #00f2fe;
  font-weight: 700;
  padding: 0.6rem 1rem;
  border-radius: 0.8rem;
  border: 1px solid rgba(0, 242, 254, 0.4);
  min-width: 3rem;
  text-align: center;
}

/* Tabla de resultados */
.table-container {
  overflow-x: auto;
  margin-bottom: 1.5rem;
  border-radius: 1rem;
  background: rgba(0, 20, 40, 0.7);
  max-height: 400px;
  overflow-y: auto;
}

.table-container::-webkit-scrollbar {
  width: 6px;
  height: 6px;
}

.table-container::-webkit-scrollbar-track {
  background: rgba(0, 0, 0, 0.3);
  border-radius: 3px;
}

.table-container::-webkit-scrollbar-thumb {
  background: linear-gradient(180deg, #00d4ff, #4facfe);
  border-radius: 3px;
}

.results-table {
  width: 100%;
  border-collapse: collapse;
  font-family: 'Orbitron', sans-serif;
}

.results-table thead {
  background: linear-gradient(135deg, #00d4ff, #4facfe);
  position: sticky;
  top: 0;
  z-index: 10;
}

.results-table th {
  padding: 1rem 0.8rem;
  text-align: center;
  font-weight: 700;
  color: white;
  font-size: 0.95rem;
  border-right: 1px solid rgba(255, 255, 255, 0.2);
  white-space: nowrap;
}

.results-table th:last-child {
  border-right: none;
}

.results-table tbody tr {
  border-bottom: 1px solid rgba(0, 212, 255, 0.2);
  transition: background-color 0.3s ease;
}

.results-table tbody tr:hover {
  background: rgba(0, 212, 255, 0.1);
}

.results-table tbody tr:last-child {
  border-bottom: none;
}

.results-table td {
  padding: 1rem 0.8rem;
  text-align: center;
  border-right: 1px solid rgba(0, 212, 255, 0.1);
}

.results-table td:last-child {
  border-right: none;
}

.index-cell {
  background: rgba(0, 212, 255, 0.1);
  color: #00f2fe;
  font-weight: 700;
  min-width: 50px;
}

.value-cell {
  color: #cceeff;
  font-weight: 600;
  min-width: 80px;
}

.operation-cell {
  color: #a0d0ff;
  font-family: 'Courier New', monospace;
  font-size: 0.85rem;
  max-width: 300px;
  min-width: 250px;
}

.operation-formula {
  background: rgba(0, 0, 0, 0.3);
  padding: 0.3rem 0.6rem;
  border-radius: 0.5rem;
  display: inline-block;
}

.result-cell {
  background: rgba(0, 242, 254, 0.15);
  color: #00f2fe;
  font-weight: 700;
  font-size: 1.1rem;
  min-width: 80px;
}

.ri-cell {
  background: rgba(79, 172, 254, 0.15);
  color: #4facfe;
  font-weight: 600;
  min-width: 100px;
}

/* Mensajes de error */
.error-message {
  background: rgba(255, 100, 100, 0.15);
  border: 1px solid rgba(255, 100, 100, 0.4);
  color: #ff6b6b;
  padding: 1rem;
  border-radius: 0.8rem;
  margin-bottom: 1.5rem;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.error-icon {
  font-size: 1.2rem;
}

.input-error {
  border-color: #ff6b6b !important;
  box-shadow: 0 0 10px rgba(255, 107, 107, 0.3) !important;
}

.field-error {
  color: #ff6b6b;
  font-size: 0.85rem;
  margin-top: 0.5rem;
  display: block;
}
.clear-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  background: rgba(255, 100, 100, 0.15);
  color: #ff6b6b;
  border: 2px solid rgba(255, 100, 100, 0.4);
  padding: 0.8rem 1.5rem;
  border-radius: 1rem;
  cursor: pointer;
  font-family: 'Orbitron', sans-serif;
  font-weight: 600;
  transition: all 0.3s ease;
  margin-top: 1.5rem;
  margin-left: auto;
}

.clear-btn:hover {
  background: rgba(255, 100, 100, 0.25);
  border-color: #ff6b6b;
  transform: translateY(-2px);
}

/* Responsive Design */
@media (max-width: 768px) {
  .main-title {
    font-size: 2.2rem;
  }

  .inputs-grid {
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }

  .space-form {
    padding: 2rem;
  }

  .results-header {
    flex-direction: column;
    align-items: flex-start;
  }

  .result-item {
    grid-template-columns: 1fr;
    text-align: center;
    gap: 0.8rem;
  }
}

@media (max-width: 480px) {
  .main-title {
    font-size: 1.8rem;
  }

  .space-form {
    padding: 1.5rem;
  }

  .content-wrapper {
    padding: 1rem 0.5rem;
  }
}
</style>
