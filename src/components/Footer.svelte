<!-- Incluí esto en el <head> de tu HTML -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">

  <footer class="footer">
    <div class="footer-left">
      <p>Visualización de Datos<br />Trabajo Final</p>
    </div>

    <div class="music-link">
      <a href="https://open.spotify.com/playlist/5KTRqBn3tfSEO7w8vlDcJq?si=Bdv5OFhzSHCFlDxG_BYpHg&pi=LYS-qQbASbOyF" target="_blank">
        <i class="fab fa-spotify"></i> Nuestra playlist
      </a>
    </div>
    
    <div class="footer-right">
      <p>Steffy - Var - Rosita</p>
      <div class="social-links">
        <a href="https://www.instagram.com/stefanyred/" target="_blank">
          <i class="fab fa-instagram"></i> @stefanyred
        </a>
        <a href="https://www.instagram.com/varvaramironov_/" target="_blank">
          <i class="fab fa-instagram"></i> @varvaramironov_
        </a>
        <a href="https://www.instagram.com/rochitagle4934/" target="_blank">
          <i class="fab fa-instagram"></i> @rochitagle4934
        </a>
      </div>
    </div>
  </footer>
  
  <style>
  .footer {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20px 40px;
    background-color: #e0fdfa;
    border-top: 2px solid #add8e6;
    box-shadow: 0 -4px 12px rgba(0, 0, 0, 0.05);
    color: black;
    font-size: 14px;
    flex-wrap: wrap;
    margin-top: 40px;
  }
  
  .footer-left,
  .footer-right {
    text-align: left;
  }
  
  .footer-right {
    text-align: right;
  }
  
  .social-links {
    margin-top: 6px;
  }
  
  .social-links a {
    display: inline-block;
    margin-left: 8px;
    text-decoration: none;
    color: #0d2de2;
    font-weight: 500;
    transition: color 0.3s ease;
  }
  
  .social-links a:first-child {
    margin-left: 0;
  }
  
  .social-links a i {
    margin-right: 5px;
    color: #E1306C; /* clásico rosa de Instagram */
  }
  
  .social-links a:hover {
    color: #ff1c68;
  }


  .music-link {
  margin-top: 10px;
}

.music-link a {
  color: #1DB954;
  text-decoration: none;
  font-weight: 200;
  transition: color 0.3s ease;
}

.music-link a i {
  margin-right: 6px;
}

.music-link a:hover {
  color: #17a34a;
}


  </style>
  