<style>
/* =========================================================
   SecNotes BESTIAIM - Página inicial personalizada
   ========================================================= */

/* Oculta el encabezado automático del tema Hacker */
body > header {
  display: none !important;
}

/* Ajusta el contenido al ocultar el header del tema */
#main_content {
  padding-top: 20px !important;
}

/* Fondo general */
body {
  background: #0b0f14 !important;
  color: #d1d5db !important;
}

/* Contenedores del tema */
.markdown-body,
.container-lg,
.wrapper,
#main_content {
  background: #0b0f14 !important;
  color: #d1d5db !important;
}

/* Contenedor principal */
.secnotes-home {
  max-width: 1080px;
  margin: 0 auto;
  padding: 20px 15px 60px 15px;
}

/* Encabezado propio */
.hero {
  text-align: center;
  padding: 30px 15px 38px 15px;
  border-bottom: 1px solid #1f2937;
  margin-bottom: 38px;
}

/* Logo Bestiaim */
.hero-logo {
  max-width: 360px;
  width: 100%;
  height: auto;
  margin: 0 auto 22px auto;
  display: block;
  border-radius: 14px;
  box-shadow: 0 0 35px rgba(56, 189, 248, 0.22);
}

/* Título principal */
.hero h1 {
  font-size: 2.8rem;
  margin: 0;
  color: #38bdf8;
  letter-spacing: 2px;
  text-shadow: 0 0 14px rgba(56, 189, 248, 0.35);
}

/* Subtítulo */
.hero p {
  color: #cbd5e1;
  font-size: 1.05rem;
  margin-top: 14px;
}

/* Bloque pequeño bajo el logo */
.repo-mini {
  margin-top: 16px;
  color: #9ca3af;
  font-size: 0.92rem;
}

/* Botón GitHub personalizado */
.repo-mini a {
  display: inline-block;
  margin-top: 12px;
  padding: 7px 14px;
  border: 1px solid #22c55e;
  border-radius: 999px;
  color: #22c55e !important;
  text-decoration: none;
  font-size: 0.86rem;
  font-weight: 600;
  background: #020617;
}

.repo-mini a:hover {
  background: #22c55e;
  color: #020617 !important;
  text-decoration: none;
}

/* Títulos de sección */
.section-title {
  color: #e5e7eb;
  font-size: 1.8rem;
  margin: 35px 0 20px 0;
  padding-bottom: 10px;
  border-bottom: 1px solid #1f2937;
}

/* Tarjetas de máquinas */
.machine-card {
  display: flex;
  gap: 24px;
  align-items: center;
  background: #111827;
  border: 1px solid #1f2937;
  border-left: 4px solid #22c55e;
  border-radius: 14px;
  padding: 18px;
  margin: 26px 0;
  box-shadow: 0 0 18px rgba(34, 197, 94, 0.08);
  transition: all 0.2s ease-in-out;
}

.machine-card:hover {
  border-color: #22c55e;
  transform: translateY(-2px);
  box-shadow: 0 0 25px rgba(34, 197, 94, 0.18);
}

/* Imagen de la tarjeta */
.machine-card img {
  width: 330px;
  height: 185px;
  object-fit: cover;
  border-radius: 10px;
  border: 1px solid #334155;
  background: #020617;
}

/* Contenido de la tarjeta */
.machine-content {
  flex: 1;
}

.machine-content h2 {
  margin-top: 0;
  margin-bottom: 10px;
  font-size: 1.55rem;
}

.machine-content h2 a {
  color: #f9fafb !important;
  text-decoration: none;
}

.machine-content h2 a:hover {
  color: #38bdf8 !important;
  text-decoration: none;
}

.machine-content p {
  color: #cbd5e1;
  line-height: 1.6;
  margin-bottom: 12px;
}

/* Etiquetas */
.tags {
  margin: 12px 0;
}

.tag {
  display: inline-block;
  background: #1e293b;
  color: #38bdf8;
  border: 1px solid #334155;
  padding: 4px 10px;
  border-radius: 999px;
  font-size: 0.82rem;
  margin-right: 6px;
  margin-bottom: 6px;
}

/* Metadata */
.meta {
  color: #9ca3af;
  font-size: 0.9rem;
  margin-top: 10px;
}

/* Botón de tarjeta */
.card-actions {
  margin-top: 14px;
}

.btn-writeup {
  display: inline-block;
  background: #16a34a;
  color: #ffffff !important;
  padding: 8px 14px;
  border-radius: 8px;
  font-weight: 600;
  text-decoration: none;
  border: 1px solid #22c55e;
}

.btn-writeup:hover {
  background: #22c55e;
  color: #020617 !important;
  text-decoration: none;
}

/* Caja de aviso */
.warning-box {
  background: #111827;
  border-left: 4px solid #f97316;
  padding: 16px;
  border-radius: 10px;
  margin-top: 38px;
  color: #d1d5db;
  line-height: 1.6;
}

.warning-box strong {
  color: #fb923c;
}

/* Ajuste responsive */
@media (max-width: 850px) {
  .machine-card {
    flex-direction: column;
    align-items: flex-start;
  }

  .machine-card img {
    width: 100%;
    height: auto;
  }

  .hero h1 {
    font-size: 2.1rem;
  }

  .hero-logo {
    max-width: 280px;
  }
}
</style>

<div class="secnotes-home">

  <div class="hero">
    <img class="hero-logo" src="assets/img/logo-bestiaim.png" alt="Logo Bestiaim">

    <h1>SecNotes Bestiaim</h1>

    <p>Write-ups, apuntes e informes de laboratorios de ciberseguridad.</p>

    <div class="repo-mini">
      <span>Repositorio personal de documentación técnica, laboratorios de pentesting y notas de ciberseguridad.</span>
      <br>
      <a href="https://github.com/bestiaim/secnotes" target="_blank">Ver repositorio en GitHub</a>
    </div>
  </div>

  <h2 class="section-title">Máquinas resueltas</h2>

  <div class="machine-card">
    <img src="assets/img/cards/nodeception.png" alt="NodeCeption">

    <div class="machine-content">
      <h2>
        <a href="maquinas/nodeception.html">NodeCeption - Pentesting Lab</a>
      </h2>

      <p>
        Resolución paso a paso de la máquina vulnerable NodeCeption. El laboratorio incluye
        reconocimiento, escaneo de puertos, enumeración web, análisis de endpoints expuestos,
        explotación mediante abuso de una automatización en n8n y escalada de privilegios local hasta root.
      </p>

      <div class="tags">
        <span class="tag">Linux</span>
        <span class="tag">n8n</span>
        <span class="tag">Web</span>
        <span class="tag">REST API</span>
        <span class="tag">Reverse Shell</span>
        <span class="tag">Privilege Escalation</span>
      </div>

      <div class="meta">Laboratorio académico · Write-up de pentesting</div>

      <div class="card-actions">
        <a class="btn-writeup" href="maquinas/nodeception.html">Ver write-up</a>
      </div>
    </div>
  </div>

  <div class="machine-card">
    <img src="assets/img/cards/cyberpunk.png" alt="Cyberpunk">

    <div class="machine-content">
      <h2>
        <a href="maquinas/cyberpunk.html">Cyberpunk - Pentesting Lab</a>
      </h2>

      <p>
        Resolución técnica de la máquina Cyberpunk. El proceso documenta FTP anónimo,
        exposición de archivos mediante Apache, carga de archivos al webroot, ejecución de PHP,
        obtención de shell inicial y escalada mediante Python Library Hijacking.
      </p>

      <div class="tags">
        <span class="tag">Linux</span>
        <span class="tag">FTP</span>
        <span class="tag">Apache</span>
        <span class="tag">PHP</span>
        <span class="tag">Webroot Upload</span>
        <span class="tag">Python Hijacking</span>
      </div>

      <div class="meta">Laboratorio académico · Write-up de pentesting</div>

      <div class="card-actions">
        <a class="btn-writeup" href="maquinas/cyberpunk.html">Ver write-up</a>
      </div>
    </div>
  </div>

  <div class="warning-box">
    <strong>Aviso de uso:</strong> todo el contenido publicado corresponde a laboratorios controlados,
    máquinas vulnerables académicas o entornos autorizados. No se debe aplicar ninguna técnica descrita
    contra sistemas reales, redes de terceros o activos sin autorización explícita.
  </div>

</div>
