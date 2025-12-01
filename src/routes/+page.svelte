<script lang="ts">
	import CalendarDoor from "$lib/components/calendarDoor.svelte";
	import MovieInfoModal from "$lib/components/modals/MovieInfoModal.svelte";
	import Snow from "$lib/components/Snow.svelte";

    interface ChristmasMovie {
        title: string;
        year: number;
        posterUrl: string;
        whereToWatch: string;
    };

    let showMovieInfoModal = $state(false);
    let movieIndex = $state(0);

    let movies: ChristmasMovie[] = $state([
        { title: "Grinch", year: 2018, posterUrl: "https://m.media-amazon.com/images/M/MV5BODQwOGFjNGEtMTg1ZS00MmY4LTg0NTctYjVlNTNjZjRmZTcxXkEyXkFqcGc@._V1_.jpg", whereToWatch: "Prime Video" },
        { title: "A Christmas Carol", year: 1843, posterUrl: "https://resizing.flixster.com/-XZAfHZM39UwaGJIFWKAE8fS0ak=/v3/t/assets/p3554929_p_v10_aa.jpg", whereToWatch: "Hulu" },
        { title: "Naquele Natal", year: 2024, posterUrl: "https://m.media-amazon.com/images/M/MV5BNDViOTM2ZTEtNzA1OC00YTc0LThmNjMtZTc1ZDQ0NDZjMTZkXkEyXkFqcGc@._V1_FMjpg_UX1000_.jpg", whereToWatch: "Netflix" },
        { title: "Arthur Christmas", year: 2011, posterUrl: "https://m.media-amazon.com/images/M/MV5BNTczMjMwMTE2OV5BMl5BanBnXkFtZTcwNDU0NTAwNw@@._V1_FMjpg_UX1000_.jpg", whereToWatch: "Prime Video" },
        { title: "Solteiro até ao Natal", year: 2021, posterUrl: "https://images.justwatch.com/poster/256213404/s718/single-all-the-way-2021.jpg", whereToWatch: "Netflix" },
        { title: "Festa de Natal da empresa", year: 2016, posterUrl: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcS25gpMv4Rm1J1DKpxRb3yQu4QsaxLTzyWGag&s", whereToWatch: "Prime Video" },
        { title: "Feliz Natal e um próspero assalto", year: 2025, posterUrl: "https://images.justwatch.com/poster/338997055/s718/feliz-natal-e-um-prospero-assalto.jpg", whereToWatch: "Netflix" },
        { title: "Um Natal ex-pecial", year: 2025, posterUrl: "https://m.media-amazon.com/images/M/MV5BMjE0NDY5Mzk2OF5BMl5BanBnXkFtZTYwNzI3MDM4._V1_FMjpg_UX1000_.jpg", whereToWatch: "Netflix" },
        { title: "Bagagem de mão", year: 2024, posterUrl: "https://images.justwatch.com/poster/322836165/s166/bagagem-de-mao", whereToWatch: "Netflix" },
        { title: "A última noitada", year: 2015, posterUrl: "https://imagens.publicocdn.com/imagens.aspx/542717?tp=KM", whereToWatch: "Prime Video" },
        { title: "Mais um Natal", year: 2023, posterUrl: "https://images.justwatch.com/poster/309989116/s718/mais-um-natal.jpg", whereToWatch: "Netflix" },
        { title: "Homem vs Bebé", year: 2025, posterUrl: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRwKctYIBcjeOuW55FCoFBKOSPuYHj8qEbgUQ&s", whereToWatch: "Netflix" },
        { title: "Calendário de Natal", year: 2018, posterUrl: "https://m.media-amazon.com/images/M/MV5BMDc2ZjU5ODctZDFkMi00ODc1LWFkNTctOGNhNTdiYWE3NGZiXkEyXkFqcGc@._V1_FMjpg_UX1000_.jpg", whereToWatch: "Netflix" },
        { title: "Próxima Natal à mesma hora", year: 2024, posterUrl: "https://m.media-amazon.com/images/M/MV5BOTRkOWQyMzktM2I3Zi00MmZmLTlhYjQtMTcwNGZjODc5OGQ4XkEyXkFqcGc@._V1_FMjpg_UX1000_.jpg", whereToWatch: "Netflix" },
        { title: "O melhor Natal de sempre", year: 2023, posterUrl: "https://m.media-amazon.com/images/M/MV5BMTM1MTdkYjUtMzQxNS00NzAxLWI3OWMtNjIyNTgyMzViNGE5XkEyXkFqcGc@._V1_.jpg", whereToWatch: "Netflix" },
        { title: "Klaus", year: 2019, posterUrl: "https://m.media-amazon.com/images/M/MV5BZDA4ODhlMzctNGRmYi00NmViLTkxZGYtZjRkNGQ5YzYwYWVlXkEyXkFqcGc@._V1_.jpg", whereToWatch: "Netflix" },
        { title: "Um castelo para o Natal", year: 2021, posterUrl: "https://br.web.img3.acsta.net/pictures/21/11/16/12/35/4355865.jpg", whereToWatch: "Netflix" },
        { title: "Um rapaz chamado Natal", year: 2021, posterUrl: "https://br.web.img3.acsta.net/pictures/21/09/15/09/05/0806353.jpg", whereToWatch: "Netflix" },
        { title: "Herança de Natal", year: 2017, posterUrl: "https://cdn.flixboss.com/80177441/posters/-2630146319504521846.jpg", whereToWatch: "Netflix" },
        { title: "Red One", year: 2024, posterUrl: "https://www.cinemundo.pt/wp-content/uploads/2024/10/TT14948432_POS_A.png", whereToWatch: "Prime Video" },
        { title: "Spirited", year: 2024, posterUrl: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQRjDkLS3tk2U5yojCa13-OBSwHpGBy9OZxGA&s", whereToWatch: "Apple TV" },
        { title: "Crónicas de Natal 1", year: 2018, posterUrl: "https://m.media-amazon.com/images/M/MV5BMTM5OGRlMmItYjE3Ny00MmNjLTk4NzQtZGI2YzJjODlmNWM2XkEyXkFqcGc@._V1_.jpg", whereToWatch: "Netflix" },
        { title: "Crónicas de Natal 2", year: 2020, posterUrl: "https://m.media-amazon.com/images/M/MV5BMWI1NWMwOWEtZDY0MS00OTU5LWEwMmMtOWFiMjE5MjUzNDUwXkEyXkFqcGc@._V1_.jpg", whereToWatch: "Netflix" },
        { title: "Sozinho em casa", year: 1990, posterUrl: "https://static.fnac-static.com/multimedia/PT/images_produits/PT/ZoomPE/2/4/0/5600304169042/tsp20110207202716/Sozinho-em-Casa.jpg", whereToWatch: "Disney +" },
        { title: "Sozinho em Casa 2", year: 1992, posterUrl: "https://imagens.publicocdn.com/imagens.aspx/756305?tp=KM", whereToWatch: "Disney +" }
    ]);
</script>

<Snow snowflakes={150} />

<div class="flex flex-col p-10 md:p-0 md:h-screen justify-center items-center gap-10">
    <h1 class="text-[#f2d8a9] text-center text-6xl md:text-7xl font-extrabold">Filmes de Natal</h1>

    <!-- movie calendar -->
    <div class="grid grid-cols-3 sm:grid-cols-4 md:grid-cols-5 gap-5">
        {#each movies as movie, index}
            <CalendarDoor dayIndex={index + 1} clickFunction={() => {
                showMovieInfoModal = true;
                movieIndex = index;
            }} />
        {/each}
    </div>
</div>

{#if showMovieInfoModal}
    <MovieInfoModal movieDay={movieIndex+1} info={movies[movieIndex]} closeModalFunction={() => {showMovieInfoModal = false}} />
{/if}