<!-- eslint-disable max-len -->
<template>

  <div class="footer-container">
    <img src="../assets/logogametracker.png" alt="logo" id="logo">
    <p id="footer-text">© 2021, Game Tracker Inc. Todos os direitos reservados. Nulla facilisi. Etiam sagittis congue tempor. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Suspendisse eu velit et ante luctus egestas. Aenean sed nisi tellus. Duis non ornare libero. Quisque suscipit maximus nisi, in gravida felis. Cras ornare mauris ac nisi congue, ut condimentum sem tincidunt. Phasellus semper tellus malesuada turpis gravida cursus. Duis nec eleifend nunc, vitae finibus elit. Aliquam eget diam vitae purus suscipit viverra.</p>

    <div id="links">
      <ul>
        <li><a href="#" target="_blank" rel="noopener">Termos de Serviço&nbsp;</a></li>
        <li><a href="#" target="_blank" rel="noopener">Política de Privacidade&nbsp;</a></li>
        <li><a href="#" target="_blank" rel="noopener">Trabalhe conosco&nbsp;</a></li>
        <li><a href="#" target="_blank" rel="noopener">Contato</a></li>
      </ul>
    </div>
  </div>

</template>

<script>

export default {
  name: 'FooterPage',

};

</script>

<style>

* {
  box-sizing: border-box;
  margin: 0;
}

a {
  text-decoration: none;
  font-family: 'Roboto', sans-serif;
  font-size: 14px;
  font-weight: 700;
  color: #FFFFFF;
}

.footer-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  color: #FFFFFF;
  background-color: #0B1641;
  max-width: 1440px;
  height: 311px;
}

#footer-text {
  font-family: 'Roboto', sans-serif;
  line-height: 24px;
  font-size: 14px;
  font-weight: 300;
}

@media screen and (max-width: 768px) {

  .footer-container {
    justify-content: center;
    align-content: center;
    text-align: center;
    padding: 0 10px;
    height: 550px;
  }

  #logo {
    margin: 20px 0 25px 0;
  }

  #links {
    margin-bottom: 10px;
  }

  li {
    list-style-type: none;
    color: #FFFFFF;
    padding: 16px 0 0 0;
    font-weight: 700;
  }
}

@media screen and (min-width: 769px) {

  .footer-container {
    padding: 0 130px 0 130px;
  }

  #logo {
    margin: 50px 0 35px 0;
  }

  #links {
    width: 100%;
  }

  ul {
    display: flex;
    align-items: center;
  }

  li {
    list-style-type: none;
    color: #FFFFFF;
    padding: 30px 50px 0 0;
    font-weight: 700;
  }
}

</style>
