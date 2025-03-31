<template>
  <div class="admin-dashboard">
    <h1 class="dashboard-title">Tableau de Bord Administrateur</h1>
    <p class="subtitle">Gestion et supervision du système Caravane Médicale</p>

    <!-- Section de résumé rapide ou de statistiques -->
    <section class="summary-section section-grid">
      <div class="summary-card card">
        <h3>Utilisateurs Actifs</h3>
        <p class="stat">{{ stats.activeUsers ?? 'N/A' }}</p>
        <router-link :to="{ name: 'UserManagement' }" class="details-link">
          Gérer les utilisateurs
        </router-link>
      </div>
      <div class="summary-card card">
        <h3>Caravanes Planifiées</h3>
        <p class="stat">{{ stats.upcomingCaravans ?? 'N/A' }}</p>
         <router-link :to="{ name: 'CaravanList' }" class="details-link">
            Voir les caravanes
         </router-link>
      </div>
      <div class="summary-card card">
        <h3>Total Consultations</h3>
        <p class="stat">{{ stats.totalConsultations ?? 'N/A' }}</p>
        <!-- Pas de lien direct évident, peut-être vers les rapports ? -->
      </div>
      <div class="summary-card card">
        <h3>Prochaine Caravane</h3>
        <p class="stat-small">
            {{ nextCaravan ? formatDate(nextCaravan.date) : 'Aucune prévue' }}
        </p>
        <p class="stat-detail" v-if="nextCaravan">
            {{ nextCaravan.location ?? 'Lieu non défini' }}
        </p>
        <router-link v-if="nextCaravan" :to="{ name: 'CaravanDetail', params: { id: nextCaravan.id } }" class="details-link">
            Voir les détails
         </router-link>
          <router-link v-else :to="{ name: 'CaravanSetup' }" class="details-link">
             Planifier une caravane
         </router-link>
      </div>
    </section>

    <!-- Section des Actions Rapides -->
    <section class="quick-actions-section card">
      <h2>Actions Rapides</h2>
      <div class="actions-grid">
        <BaseButton variant="secondary" @click="goToUserManagement">
           <i class="fas fa-users-cog"></i> Gérer les Utilisateurs
        </BaseButton>
        <BaseButton variant="secondary" @click="goToCaravanSetup">
          <i class="fas fa-plus-circle"></i> Planifier une Nouvelle Caravane
        </BaseButton>
        <BaseButton variant="secondary" @click="goToReporting">
            <i class="fas fa-chart-line"></i> Consulter les Rapports
        </BaseButton>
         <!-- Ajoutez d'autres boutons pour des actions courantes -->
          <!-- <BaseButton variant="secondary" @click="goToSystemSettings">
            <i class="fas fa-cogs"></i> Paramètres Système
        </BaseButton> -->
      </div>
    </section>

    <!-- Section Activité Récente (optionnel) -->
    <!-- <section class="recent-activity-section card">
      <h2>Activité Récente</h2>
      <ul v-if="recentActivities.length > 0">
        <li v-for="activity in recentActivities" :key="activity.id">
            {{ formatDate(activity.timestamp) }} - {{ activity.description }}
        </li>
      </ul>
      <p v-else>Aucune activité récente enregistrée.</p>
    </section> -->

  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRouter } from 'vue-router';
// Importez des services si nécessaire pour récupérer les statistiques
// import adminService from '@/services/admin.service.js';
import BaseButton from '@/components/shared/BaseButton.vue';
// Si vous utilisez FontAwesome ou une autre bibliothèque d'icônes
// import '@fortawesome/fontawesome-free/css/all.css'; // Ou configurez globalement

const router = useRouter();

// Données de statistiques (simulées ou à charger)
const stats = ref({
  activeUsers: null,
  upcomingCaravans: null,
  totalConsultations: null,
});

const nextCaravan = ref(null); // Pour stocker les infos de la prochaine caravane

// const recentActivities = ref([]); // Pour l'activité récente

// Fonction utilitaire simple pour formater les dates
const formatDate = (dateString) => {
    if (!dateString) return '';
    const options = { year: 'numeric', month: 'long', day: 'numeric' };
    try {
      return new Date(dateString).toLocaleDateString('fr-FR', options);
    } catch(e) {
        return dateString; // Retourne la string si le formatage échoue
    }
};


// Charger les données au montage du composant
onMounted(async () => {
  console.log('Chargement des données du tableau de bord admin...');
  // Remplacez par des appels réels aux services pour obtenir les données
  try {
    // Exemple de chargement simulé
    await new Promise(resolve => setTimeout(resolve, 500));
    stats.value = {
      activeUsers: 42,
      upcomingCaravans: 3,
      totalConsultations: 1258,
    };
     nextCaravan.value = {
        id: 'caravan-xyz',
        date: new Date(Date.now() + 7 * 24 * 60 * 60 * 1000).toISOString(), // Dans 7 jours
        location: 'Village de Tarmigt'
    }
    // const fetchedStats = await adminService.getDashboardStats();
    // stats.value = fetchedStats;
    // const fetchedNextCaravan = await adminService.getNextCaravan();
    // nextCaravan.value = fetchedNextCaravan;
    // const fetchedActivities = await adminService.getRecentActivities();
    // recentActivities.value = fetchedActivities;
    console.log('Données chargées.');
  } catch (error) {
    console.error('Erreur lors du chargement des données du tableau de bord:', error);
    // Afficher une notification d'erreur à l'utilisateur si nécessaire
  }
});

// Fonctions de navigation
function goToUserManagement() {
  router.push({ name: 'UserManagement' }); // Assurez-vous que la route 'UserManagement' existe
}

function goToCaravanSetup() {
  router.push({ name: 'CaravanSetup' }); // Assurez-vous que la route 'CaravanSetup' existe
}

function goToReporting() {
    router.push({ name: 'ReportsDashboard' }); // Assurez-vous que la route 'ReportsDashboard' existe
}

// function goToSystemSettings() {
//  // router.push({ name: 'SystemSettings' }); // Si une telle route existe
// }

</script>

<style scoped>
.admin-dashboard {
  padding: 25px;
  max-width: 1200px;
  margin: 0 auto;
}

.dashboard-title {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 10px;
}

.subtitle {
    text-align: center;
    color: #7f8c8d;
    margin-bottom: 30px;
}

.card {
  background-color: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.08);
}

/* Section Résumé */
.summary-section {
  margin-bottom: 30px;
}
.section-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); /* Responsive grid */
  gap: 20px;
}

.summary-card h3 {
  margin-top: 0;
  margin-bottom: 10px;
  color: #3498db; /* Bleu pour les titres des cartes */
  font-size: 1.1em;
}

.summary-card .stat {
  font-size: 2.2em;
  font-weight: bold;
  color: #2c3e50;
  margin-bottom: 10px;
  line-height: 1.1;
}
.summary-card .stat-small {
  font-size: 1.2em;
  font-weight: bold;
  color: #2c3e50;
  margin-bottom: 5px;
}
.summary-card .stat-detail {
  font-size: 0.9em;
  color: #555;
  margin-bottom: 15px;
}

.summary-card .details-link {
  font-size: 0.9em;
  color: #3498db;
  text-decoration: none;
  display: block; /* Prend toute la largeur */
  text-align: right;
}

.summary-card .details-link:hover {
  text-decoration: underline;
}


/* Section Actions Rapides */
.quick-actions-section {
  margin-bottom: 30px;
}
.quick-actions-section h2 {
    margin-top: 0;
    margin-bottom: 20px;
    color: #555;
}

.actions-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 15px;
}
/* Style pour icônes dans les boutons (si vous utilisez FontAwesome ou similaire) */
.actions-grid i {
    margin-right: 8px;
}


/* Section Activité Récente */
/* .recent-activity-section h2 {
    margin-top: 0;
    margin-bottom: 15px;
    color: #555;
}
.recent-activity-section ul {
    list-style: none;
    padding: 0;
    margin: 0;
    max-height: 300px;
    overflow-y: auto;
}
.recent-activity-section li {
    padding: 8px 0;
    border-bottom: 1px dashed #eee;
    font-size: 0.95em;
    color: #333;
}
.recent-activity-section li:last-child {
    border-bottom: none;
} */

</style>
