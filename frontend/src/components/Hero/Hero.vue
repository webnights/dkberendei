<template>
    <section class="hero">
        <div class="container">
            <div class="hero__inner">
                <div class="hero__content">
                    <h1>Дом культуры
                        <span style="display: block;">Берендей</span>
                    </h1>
                    <p>
                        Добро пожаловать в Дом Культуры "Берендей"!

                        Здесь вас ждут яркие события: концерты, спектакли, выставки, фестивали, мастер-классы и творческие встречи.

                        Раскройте свой талант в наших студиях и кружках: вокал, танцы, театр, декоративно-прикладное искусство и многое другое.

                        "Берендей" - это пространство для творчества, общения и вдохновения.

                        Следите за афишей на нашем сайте и в социальных сетях!
                    </p>
                    <RouterLink to="/products">
                        <Button :content="'Афиша'" :type="'button'"/>
                    </RouterLink>
                </div>
                <div class="hero__image">
                    <img src="/src/assets/images/hero.jpg">
                </div>
            </div>
        </div>
    </section>
</template>
<script>
    import Button from '/src/components/Button/Button.vue' 
    export default{
        components: {Button},
        methods: {
        scrollToBuses() {
        const busesSection = document.querySelector('.products')
        if (busesSection) {
            busesSection.scrollIntoView({ behavior: 'smooth', alignToTop: true });
        }
        }
    }
    }
</script>
