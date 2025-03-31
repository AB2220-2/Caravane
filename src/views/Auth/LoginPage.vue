<template>
  <div class="login-page">
    <div class="login-container">
      <img
        v-if="logoUrl"
        :src="logoUrl"
        alt="Logo Caravane Médicale"
        class="login-logo" />
      <h1>Connexion</h1>
      <p class="subtitle">Accédez à votre espace Caravane Médicale</p>

      <form @submit.prevent="handleLogin" class="login-form">
        <div class="form-group">
          <label for="username">Identifiant</label>
          <input
            type="text"
            id="username"
            v-model="username"
            required
            placeholder="Votre nom d'utilisateur ou email"
            :disabled="isLoading"
            aria-required="true"
           />
        </div>

        <div class="form-group">
          <label for="password">Mot de passe</label>
          <input
            type="password"
            id="password"
            v-model="password"
            required
            placeholder="********"
            :disabled="isLoading"
            aria-required="true"
           />
           <!-- Optionnel: lien mot de passe oublié -->
           <!-- <a href="#" class="forgot-password">Mot de passe oublié ?</a> -->
        </div>

        <div v-if="errorMessage" class="error-message">
          {{ errorMessage }}
        </div>

        <BaseButton
          type="submit"
          :disabled="isLoading"
          variant="primary"
          class="login-button"
        >
          <span v-if="isLoading">Connexion en cours...</span>
          <span v-else>Se connecter</span>
        </BaseButton>

      </form>
    </div>
     <!-- Peut-être un pied de page simple -->
     <footer class="login-footer">
        © {{ new Date().getFullYear() }} Caravane Médicale. Tous droits réservés.
      </footer>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
// Importez votre service d'authentification ou les actions du store
// import authService from '@/services/auth.service'; // Exemple avec service
// import { useAuthStore } from '@/store/auth.module'; // Exemple avec Pinia Store
import BaseButton from '@/components/shared/BaseButton.vue';

// Si vous utilisez l'image de la bannière comme logo (à adapter)
// Vous devrez peut-être créer un logo plus petit/adapté
import logoAsset from '@/assets/images/banner-caravane.png'; // Attention : utiliser la vraie bannière ici est probablement trop grand. Mettez le chemin vers un VRAI logo. S'il n'y en a pas, mettez à null.
const logoUrl = ref(logoAsset); // Ou ref(null) si pas de logo pour l'instant

// Variables réactives pour les champs du formulaire et l'état
const username = ref('');
const password = ref('');
const isLoading = ref(false);
const errorMessage = ref('');

// Accès au routeur pour la redirection après connexion réussie
const router = useRouter();

// (Si utilisation d'un store Pinia)
// const authStore = useAuthStore();

// Fonction pour gérer la soumission du formulaire
const handleLogin = async () => {
  // Réinitialiser les erreurs et activer le chargement
  errorMessage.value = '';
  isLoading.value = true;

  try {
    // --- Logique d'authentification ---
    // Remplacez ceci par votre appel réel au service ou à l'action du store
    console.log('Tentative de connexion avec:', username.value);

    // Exemple (à remplacer !) :
    // Simule un appel API qui réussit après 1 seconde
    await new Promise(resolve => setTimeout(resolve, 1000));
    const fakeUserData = { id: 1, username: username.value, role: 'Médecin Généraliste' }; // Simule des données utilisateur reçues
    console.log('Connexion réussie (simulation)', fakeUserData);

    // --- Si vous utilisez un service directement ---
    // const userData = await authService.login({
    //   username: username.value,
    //   password: password.value
    // });
    // Stocker les données utilisateur / token (peut-être dans le localStorage ou via le store)

    // --- Si vous utilisez un store (ex: Pinia) ---
    // await authStore.login({
    //   username: username.value,
    //   password: password.value
    // });

    // Redirection vers la page appropriée après connexion réussie
    // Adaptez le nom de la route en fonction de votre configuration router/index.js
    router.push({ name: 'ReceptionDashboard' }); // Ou rediriger selon le rôle de l'utilisateur

  } catch (error) {
    console.error('Erreur de connexion:', error);
    // Afficher un message d'erreur à l'utilisateur
    // Essayez d'extraire un message spécifique de l'erreur si possible
    errorMessage.value = error.response?.data?.message || "Échec de la connexion. Vérifiez vos identifiants.";

  } finally {
    // Désactiver l'état de chargement, que la connexion ait réussi ou échoué
    isLoading.value = false;
  }
};
</script>

<style scoped>
.login-page {
  display: flex;
  flex-direction: column;
  justify-content: center; /* Centre verticalement dans la fenêtre */
  align-items: center; /* Centre horizontalement */
  min-height: 100vh; /* Prend au moins toute la hauteur de la vue */
  background-color: #f4f7f6; /* Couleur de fond douce */
  padding: 20px;
}

.login-container {
  background-color: #fff;
  padding: 30px 40px;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 400px; /* Limite la largeur du formulaire */
  text-align: center;
  margin-bottom: 20px; /* Espace avant le footer */
}

.login-logo {
  max-width: 150px; /* Ajustez selon la taille de votre logo */
  margin-bottom: 20px;
  /* S'assurer que l'image n'est pas déformée si elle est rectangulaire */
  height: auto;
  display: block; /* Pour centrer avec margin auto */
  margin-left: auto;
  margin-right: auto;
}


h1 {
  color: #333;
  margin-bottom: 10px;
}

.subtitle {
    color: #666;
    margin-bottom: 30px;
}

.login-form {
  display: flex;
  flex-direction: column;
  gap: 20px; /* Espace entre les groupes de formulaire */
  text-align: left; /* Aligner les labels à gauche */
}

.form-group {
  display: flex;
  flex-direction: column;
}

.form-group label {
  margin-bottom: 5px;
  font-weight: bold;
  color: #555;
}

.form-group input {
  padding: 10px 12px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 1em;
}

.form-group input:focus {
  outline: none;
  border-color: #3498db; /* Mettre en évidence lors de la sélection */
  box-shadow: 0 0 0 2px rgba(52, 152, 219, 0.2);
}

.form-group input:disabled {
  background-color: #eee;
  cursor: not-allowed;
}

/* .forgot-password {
  align-self: flex-end;
  font-size: 0.9em;
  color: #3498db;
  text-decoration: none;
  margin-top: -10px;
}
.forgot-password:hover {
    text-decoration: underline;
} */

.error-message {
  color: #e74c3c; /* Rouge pour les erreurs */
  background-color: #fdeded;
  border: 1px solid #f6c4c4;
  border-radius: 4px;
  padding: 10px;
  text-align: center;
  font-size: 0.9em;
}

.login-button {
  margin-top: 10px; /* Espace au-dessus du bouton */
  width: 100%; /* Bouton pleine largeur */
}

.login-footer {
    font-size: 0.8em;
    color: #888;
    text-align: center;
}
</style>
