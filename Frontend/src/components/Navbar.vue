<template>
  <nav class="navbar">
    <div class="brand-container">
      <router-link to="/" class="brand">Rand#</router-link>
    </div>
    <ul class="navbar-menu">
      <li><router-link to="/" class="nav-link">Inicio</router-link></li>
      <li class="dropdown">
        <a class="nav-link"
          >Generadores
          <span class="arrow">▾</span>
        </a>
        <ul class="dropdown-menu">
          <li><router-link to="/lineal">Lineal Congruencial</router-link></li>
          <li><router-link to="/multiplicativo">Congruencial multiplicativa</router-link></li>
        </ul>
      </li>
    </ul>
  </nav>
</template>

<script>
export default {
  name: 'Navbar',
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@500;700&display=swap');

.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  width: 100vw;
  height: 90px;
  padding: 0 2rem;
  margin: 0;
  display: flex;
  align-items: center;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.7);
  z-index: 50;

  background: linear-gradient(120deg, #0f2027, #203a43, #2c5364);
  background-size: 400% 400%;
  animation: gradientShift 15s ease infinite;

  box-sizing: border-box;
}

.arrow {
  display: inline-block;
  margin-left: 0.3rem;
  transition: transform 0.3s ease;
  transform: rotate(180deg);
}

@keyframes gradientShift {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

.brand-container {
  flex: 1;
}

.brand {
  font-family: 'Orbitron', sans-serif;
  font-size: 2rem;
  font-weight: 700;
  color: #12a0bd;
  text-decoration: none;
  letter-spacing: 2px;
  text-shadow:
    0 0 5px #0899b6,
    0 0 10px #0899b6;
}

.navbar-menu {
  list-style: none;
  display: flex;
  gap: 2.5rem;
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  margin: 0;
  padding: 0;
  align-items: center;
}

.navbar-menu li {
  position: relative;
}

.nav-link {
  font-family: 'Orbitron', sans-serif;
  font-size: 1.1rem;
  color: white;
  text-decoration: none;
  transition: 0.3s ease;
  display: flex;
  align-items: center;
  padding: 1rem 0;
}

.nav-link:hover {
  color: #00d4ff;
  text-shadow:
    0 0 5px #00d4ff,
    0 0 10px #00d4ff;
}

.dropdown {
  position: relative;
}

.dropdown::after {
  content: '';
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  width: 100%;
  height: 15px;
  background: transparent;
  z-index: 10;
}

.dropdown-menu {
  display: none;
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  background-color: rgba(20, 20, 30, 0.95);
  border-radius: 8px;
  padding: 0.5rem 0;
  min-width: 220px;
  box-shadow: 0 6px 15px rgba(0, 0, 0, 0.5);
  list-style: none;
  overflow: hidden;
  opacity: 0;
  margin-top: 15px;
  transition:
    opacity 0.3s ease,
    transform 0.3s ease;
  pointer-events: none;
  z-index: 20;
}

.dropdown:hover .dropdown-menu {
  display: block;
  opacity: 1;
  transform: translateX(-50%) translateY(0);
  pointer-events: auto;
}

.dropdown:hover .arrow {
  transform: rotate(0deg);
}

.dropdown-menu li {
  margin: 0;
}

.dropdown-menu li a {
  display: block;
  padding: 0.6rem 1.5rem;
  color: #fff;
  text-decoration: none;
  font-family: 'Orbitron', sans-serif;
  font-weight: 500;
  transition: 0.2s ease;
}

.dropdown-menu li a:hover {
  background-color: rgba(0, 212, 255, 0.2);
  color: #00d4ff;
  text-shadow: 0 0 5px #00d4ff;
}

.navbar:hover {
  box-shadow: 0 6px 25px rgba(0, 212, 255, 0.5);
}
</style>
