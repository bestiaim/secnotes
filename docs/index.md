<style>
/* ===== SecNotes Bestiaim - Home ===== */

body {
  background: #0b0f14;
  color: #d1d5db;
}

.markdown-body,
.container-lg,
.wrapper {
  background: #0b0f14;
  color: #d1d5db;
}

.secnotes-home {
  max-width: 1050px;
  margin: 0 auto;
  padding: 20px 10px 50px 10px;
}

/* Header */
.hero {
  text-align: center;
  padding: 35px 15px 30px 15px;
  border-bottom: 1px solid #1f2937;
  margin-bottom: 35px;
}

.hero-logo {
  max-width: 360px;
  width: 100%;
  height: auto;
  margin: 0 auto 20px auto;
  display: block;
  border-radius: 14px;
  box-shadow: 0 0 35px rgba(56, 189, 248, 0.18);
}

.hero h1 {
  font-size: 2.8rem;
  margin: 0;
  color: #38bdf8;
  letter-spacing: 1px;
}

.hero p {
  color: #9ca3af;
  font-size: 1.08rem;
  margin-top: 12px;
}

/* Titles */
.section-title {
  color: #e5e7eb;
  font-size: 1.8rem;
  margin: 35px 0 20px 0;
  padding-bottom: 10px;
  border-bottom: 1px solid #1f2937;
}

/* Cards */
.machine-card {
  display: flex;
  gap: 24px;
  align-items: center;
  background: #111827;
  border: 1px solid #1f2937;
  border-left: 4px solid #22c55e;
  border-radius: 14px;
  padding: 18px;
  margin: 24px 0;
  box-shadow: 0 0 18px rgba(34, 197, 94, 0.08);
  transition: all 0.2s ease-in-out;
}

.machine-card:hover {
  border-color: #22c55e;
  transform: translateY(-2px);
  box-shadow: 0 0 25px rgba(34, 197, 94, 0.18);
}

.machine-card img {
  width: 330px;
  height: 185px;
  object-fit: cover;
  border-radius: 10px;
  border: 1px solid #334155;
  background: #020617;
}

.machine-content {
  flex: 1;
}

.machine-content h2 {
  margin-top: 0;
  margin-bottom: 10px;
  font-size: 1.55rem;
}

.machine-content h2 a {
  color: #f9fafb;
  text-decoration: none;
}

.machine-content h2 a:hover {
  color: #38bdf8;
}

.machine-content p {
  color: #cbd5e1;
  line-height: 1.6;
  margin-bottom: 12px;
}

/* Tags */
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

/* Buttons */
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

/* Metadata */
.meta {
  color: #9ca3af;
  font-size: 0.9rem;
  margin-top: 10px;
}

/* Warning */
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

/* Responsive */
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
        explotación mediante abuso de una automatización en n8n y escalada de privilegios local.
      </p>

      <div class="tags">
        <span class="tag">Linux</span>
        <span class="tag">n8n</span>
        <span class="tag">Web</span>
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
