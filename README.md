<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
</head>
<body>

<h1>HireMatch</h1>

<p>
  <strong>HireMatch</strong> es una aplicación móvil de emparejamiento laboral (job matching) diseñada bajo un enfoque
  <em>mobile-first</em>, que conecta postulantes con empresas mediante una interfaz tipo
  <strong>Tinder-like</strong> basada en tarjetas y gestos de deslizamiento.
</p>

<div class="note">
  <strong>Repositorio del Frontend:</strong><br>
  👉 <em>https://github.com/Fabri0607/HireMatch-Api</em>
</div>

<h2>Propósito y Alcance</h2>

<p>
  Este documento proporciona una visión general del sistema <strong>HireMatch</strong>, cubriendo su arquitectura,
  tecnologías principales, flujos de usuario y componentes clave.
  Para información detallada sobre subsistemas específicos, se recomienda consultar:
</p>

<ul>
  <li>Configuración inicial y entorno de desarrollo</li>
  <li>Patrones de arquitectura y diseño del sistema</li>
  <li>Capa de servicios de la API</li>
  <li>Flujos de autenticación y onboarding</li>
  <li>Experiencia del postulante y de la empresa</li>
</ul>

<h2>¿Qué es HireMatch?</h2>

<p>
  HireMatch es una aplicación de emparejamiento laboral que implementa una
  <strong>arquitectura de doble tipo de usuario</strong>:
</p>

<ul>
  <li>
    <strong>Postulantes:</strong> exploran ofertas de empleo deslizando tarjetas y aplicando
    acciones de <em>like</em> o <em>super-like</em>.
  </li>
  <li>
    <strong>Empresas:</strong> crean, administran y eliminan ofertas de trabajo, además de
    monitorear la interacción de los postulantes.
  </li>
</ul>

<p>
  La aplicación está construida como una solución móvil multiplataforma para
  <strong>iOS y Android</strong>, utilizando un frontend en
  <strong>React Native</strong> y un backend basado en
  <strong>Java Spring Boot</strong>.
</p>

<h2>Tecnologías Principales</h2>

<table>
  <thead>
    <tr>
      <th>Tecnología</th>
      <th>Propósito</th>
      <th>Versión</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Expo</td>
      <td>Plataforma de desarrollo y build</td>
      <td>54.0.4</td>
    </tr>
    <tr>
      <td>React Native</td>
      <td>Framework móvil multiplataforma</td>
      <td>0.81.4</td>
    </tr>
    <tr>
      <td>React</td>
      <td>Librería de UI</td>
      <td>19.1.0</td>
    </tr>
    <tr>
      <td>TypeScript</td>
      <td>Tipado estático</td>
      <td>~5.8.3</td>
    </tr>
    <tr>
      <td>expo-router</td>
      <td>Ruteo basado en archivos</td>
      <td>~6.0.2</td>
    </tr>
    <tr>
      <td>Axios</td>
      <td>Cliente HTTP</td>
      <td>^1.11.0</td>
    </tr>
    <tr>
      <td>AsyncStorage</td>
      <td>Persistencia local</td>
      <td>2.2.0</td>
    </tr>
    <tr>
      <td>NativeWind</td>
      <td>Tailwind CSS para React Native</td>
      <td>^4.1.23</td>
    </tr>
    <tr>
      <td>react-native-deck-swiper</td>
      <td>Componente de swipe</td>
      <td>^2.0.19</td>
    </tr>
  </tbody>
</table>

<p>
  El backend expone una API REST alojada en <code>http://localhost:8080</code> y utiliza
  <strong>autenticación basada en JWT</strong>.
</p>

<h2>Arquitectura del Sistema</h2>

<p>
  HireMatch sigue una arquitectura en capas con separación clara de responsabilidades:
</p>

<ul>
  <li><strong>Root Layout:</strong> punto de entrada de la aplicación y control inicial de autenticación</li>
  <li><strong>Capa de Autenticación:</strong> registro, login y verificación de usuarios</li>
  <li><strong>Capa de Servicios API:</strong> comunicación centralizada con el backend</li>
  <li><strong>Rutas Protegidas:</strong> experiencias separadas según tipo de perfil</li>
  <li><strong>Persistencia Local:</strong> manejo de sesión mediante AsyncStorage</li>
</ul>

<h2>Tipos de Usuario y Flujos</h2>

<h3>Postulantes (<code>tipo_perfil = 'postulante'</code>)</h3>

<ul>
  <li>Interfaz principal: visor de ofertas con swipe</li>
  <li>Acciones principales:
    <ul>
      <li><code>likeJobOffer(ofertaId)</code></li>
      <li><code>superLikeJobOffer(ofertaId)</code></li>
      <li><code>getUserApplications(estado?)</code></li>
    </ul>
  </li>
  <li>Navegación por pestañas: Home, Applications y Profile</li>
</ul>

<h3>Empresas (<code>tipo_perfil = 'empresa'</code>)</h3>

<ul>
  <li>Gestión de ofertas laborales</li>
  <li>Acciones principales:
    <ul>
      <li><code>createJobOffer(data)</code></li>
      <li><code>updateJobOffer(id, data)</code></li>
      <li><code>deleteJobOffer(id)</code></li>
      <li><code>getJobOffersByCompany(empresaId)</code></li>
    </ul>
  </li>
  <li>Navegación específica para empresas</li>
</ul>

<h2>Componentes Clave</h2>

<h3>1. Capa de Servicios API</h3>

<p>
  Ubicada en <code>app/services/api.ts</code>, es el componente más crítico del sistema.
  Centraliza la comunicación con el backend y gestiona:
</p>

<ul>
  <li>Autenticación y registro</li>
  <li>Gestión de perfiles</li>
  <li>CRUD de ofertas laborales</li>
  <li>Emparejamiento y postulaciones</li>
</ul>

<h3>2. Sistema de Navegación</h3>

<p>
  Implementado mediante <strong>expo-router</strong>, usando ruteo basado en archivos y
  layouts protegidos por autenticación.
</p>

<h3>3. Autenticación y Sesión</h3>

<ul>
  <li>Tokens JWT almacenados en AsyncStorage</li>
  <li>Inyección automática del token en cada request</li>
  <li>Persistencia de sesión entre reinicios</li>
</ul>

<h3>4. Validación de Datos</h3>

<p>
  Se implementa validación del lado del cliente para perfiles, ofertas laborales
  y formatos de datos como teléfonos, URLs y enums.
</p>

<h2>Inicio de la Aplicación</h2>

<p>
  El flujo de arranque incluye:
</p>

<ul>
  <li>Carga de fuentes personalizadas</li>
  <li>Pantalla splash de 2 segundos</li>
  <li>Verificación de sesión activa</li>
  <li>Redirección según estado de autenticación</li>
</ul>

<h2>Resumen</h2>

<p>
  HireMatch es una aplicación móvil moderna de emparejamiento laboral que combina:
</p>

<ul>
  <li>Arquitectura modular y escalable</li>
  <li>Experiencias diferenciadas por tipo de usuario</li>
  <li>Comunicación segura mediante JWT</li>
  <li>Navegación tipada y basada en archivos</li>
  <li>Persistencia local para una experiencia fluida</li>
</ul>

<p>
  Este diseño permite una base sólida para futuras extensiones y mejoras del sistema.
</p>

</body>
</html>
