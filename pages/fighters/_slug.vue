<template>
    <div v-if="fighter" class="fighter-page">
        <div class="fighter-profile">
            <div class="fighter-details">
                <h1 class="fighter-name">{{ fighter.name }}</h1>
                <div class="info-section">
                    <h2 class="fighter-belt">Ceinture: {{ fighter.belt }}</h2>
                    <div class="fighter-info">
                        <div class="fighter-country">
                            <img :src="fighter.country_player.url" :alt="`Drapeau de ${fighter.country}`">
                            {{ fighter.country}}
                        </div>
                        <div class="fighter-weight">Poids: {{ fighter.weight }}</div>
                        <div class="fighter-birth">Date de naissance: {{ fighter.birth }}</div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    async asyncData({ params, $axios }) {
        try {
            const { data } = await $axios.get(`/api/fighters?filters[slug][$eq]=${params.slug}&populate=*`);
            console.log('Combattant récupéré:', data.data[0]);
            return { fighter: data.data[0] }; 
        } catch (error) {
            console.error('Erreur lors de la récupération des détails du combattant:', error);
            return { fighter: null };
        }
    }
}
</script>

<style scoped>
/* Styles pour le composant FighterProfile */
.fighter-page {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

.fighter-profile {
    background-color: #f0f0f0;
    border-radius: 10px;
    padding: 20px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.fighter-details {
    text-align: center;
}

.fighter-name {
    margin-top: 0;
    color: #333;
}

.info-section {
    margin-top: 20px;
}

.fighter-belt {
    color: #007bff;
}

.fighter-info {
    margin-top: 10px;
}

.fighter-country img {
    width: 30px;
    margin-right: 10px;
    vertical-align: middle;
}

.fighter-country {
    margin-top: 10px;
    font-weight: bold;
}

.fighter-weight,
.fighter-birth {
    margin-top: 5px;
    color: #666;
}
</style>
