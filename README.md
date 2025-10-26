<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SceneSeeker - Movie Recommendations</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        :root {
            --primary-color: #ff6b6b;
            --secondary-color: #4ecdc4;
            --dark-bg: #121212;
            --darker-bg: #1e1e1e;
            --light-text: #ffffff;
            --gray-text: #b3b3b3;
            --card-bg: #2a2a2a;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Poppins', sans-serif;
            background-color: var(--dark-bg);
            color: var(--light-text);
            line-height: 1.6;
            overflow-x: hidden;
        }
        
        /* Loading Spinner */
        .loading {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 200px;
            color: var(--primary-color);
        }
        
        .spinner {
            border: 4px solid var(--gray-text);
            border-top: 4px solid var(--primary-color);
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
        }
        
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        /* Hero Section */
        .hero {
            background: linear-gradient(135deg, var(--primary-color), var(--secondary-color));
            padding: 100px 20px;
            text-align: center;
            position: relative;
            overflow: hidden;
        }
        
        .hero::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: url('https://via.placeholder.com/1920x1080/000000/ffffff?text=Movie+Background') center/cover;
            opacity: 0.1;
            z-index: 1;
        }
        
        .hero-content {
            position: relative;
            z-index: 2;
        }
        
        .hero h1 {
            font-size: 3rem;
            font-weight: 700;
            margin-bottom: 20px;
            animation: fadeInUp 1s ease-out;
        }
        
        .hero p {
            font-size: 1.2rem;
            margin-bottom: 40px;
            animation: fadeInUp 1s ease-out 0.2s both;
        }
        
        .search-bar {
            max-width: 600px;
            margin: 0 auto;
            position: relative;
            animation: fadeInUp 1s ease-out 0.4s both;
        }
        
        .search-bar input {
            width: 100%;
            padding: 15px 50px 15px 20px;
            font-size: 18px;
            border: none;
            border-radius: 50px;
            background-color: rgba(255, 255, 255, 0.9);
            color: var(--dark-bg);
            transition: all 0.3s ease;
        }
        
        .search-bar input:focus {
            outline: none;
            box-shadow: 0 0 20px rgba(255, 107, 107, 0.5);
            background-color: white;
        }
        
        .search-bar .fa-search {
            position: absolute;
            right: 20px;
            top: 50%;
            transform: translateY(-50%);
            color: var(--dark-bg);
        }
        
        /* Carousel */
        .carousel-section {
            padding: 60px 20px;
            background-color: var(--darker-bg);
        }
        
        .carousel-section h2 {
            text-align: center;
            font-size: 2.5rem;
            margin-bottom: 40px;
            color: var(--primary-color);
        }
        
        .carousel {
            display: flex;
            overflow-x: auto;
            scroll-snap-type: x mandatory;
            gap: 20px;
            padding: 20px 0;
            scrollbar-width: none;
            -ms-overflow-style: none;
        }
        
        .carousel::-webkit-scrollbar {
            display: none;
        }
        
        .movie-card {
            flex: 0 0 250px;
            scroll-snap-align: start;
            background-color: var(--card-bg);
            border-radius: 15px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
            cursor: pointer;
        }
        
        .movie-card:hover {
            transform: translateY(-10px);
            box-shadow: 0 20px 40px rgba(0,0,0,0.5);
        }
        
        .movie-card img {
            width: 100%;
            height: 300px;
            object-fit: cover;
        }
        
        .movie-card .content {
            padding: 15px;
        }
        
        .movie-card h3 {
            font-size: 1.1rem;
            margin-bottom: 5px;
        }
        
        .movie-card p {
            color: var(--gray-text);
            font-size: 0.9rem;
        }
        
        /* Movie Details */
        .movie-details {
            display: none;
            padding: 60px 20px;
            background-color: var(--dark-bg);
            min-height: 100vh;
        }
        
        .details-container {
            max-width: 1200px;
            margin: 0 auto;
            display: grid;
            grid-template-columns: 1fr 2fr;
            gap: 40px;
        }
        
        .poster {
            border-radius: 15px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
        }
        
        .poster img {
            width: 100%;
            display: block;
        }
        
        .info h1 {
            font-size: 2.5rem;
            margin-bottom: 20px;
        }
        
        .rating {
            display: flex;
            align-items: center;
            margin-bottom: 20px;
        }
        
        .rating .fa-star {
            color: var(--primary-color);
            margin-right: 10px;
        }
        
        .overview {
            margin-bottom: 30px;
            font-size: 1.1rem;
            line-height: 1.8;
        }
        
        .cast-crew, .related-movies {
            margin-top: 40px;
        }
        
        .cast-crew h3, .related-movies h3 {
            font-size: 1.8rem;
            margin-bottom: 20px;
            color: var(--primary-color);
        }
        
        .cast-list, .related-list {
            display: flex;
            overflow-x: auto;
            gap: 20px;
            padding: 20px 0;
        }
        
        .cast-member, .related-movie {
            flex: 0 0 150px;
            text-align: center;
        }
        
        .cast-member img, .related-movie img {
            width: 100px;
            height: 100px;
            border-radius: 50%;
            object-fit: cover;
            margin-bottom: 10px;
        }
        
        .back-btn {
            background-color: var(--primary-color);
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 25px;
            font-size: 16px;
            cursor: pointer;
            transition: background-color 0.3s ease;
            margin-bottom: 30px;
        }
        
        .back-btn:hover {
            background-color: #ff5252;
        }
        
        /* Error Message */
        .error {
            text-align: center;
            color: var(--primary-color);
            font-size: 1.2rem;
            padding: 40px;
        }
        
        /* Animations */
        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        
        /* Responsive */
        @media (max-width: 768px) {
            .hero h1 {
                font-size: 2rem;
            }
            .details-container {
                grid-template-columns: 1fr;
            }
            .movie-card {
                flex: 0 0 200px;
            }
        }
    </style>
</head>
<body>
    <section class="hero">
        <div class="hero-content">
            <h1><i class="fas fa-film"></i> SceneSeeker</h1>
            <p>Discover your next favorite movie with our curated recommendations</p>
            <div class="search-bar">
                <input type="text" id="searchInput" placeholder="Search for movies...">
                <i class="fas fa-search"></i>
            </div>
        </div>
    </section>
    
    <section class="carousel-section">
        <h2>Latest Movies</h2>
        <div id="moviesCarousel" class="carousel">
            <div class="loading">
                <div class="spinner"></div>
                <p>Loading movies...</p>
            </div>
        </div>
    </section>
    
    <div id="movieDetails" class="movie-details">
        <div class="container">
            <button class="back-btn" onclick="showHome()"><i class="fas fa-arrow-left"></i> Back to Home</button>
            <div id="detailsContent" class="details-container">
                <!-- Movie details will be loaded here -->
            </div>
        </div>
    </div>
    
    <script>
        const apiKey = 'YOUR_TMDB_API_KEY_HERE'; // Replace with your actual TMDB API key
        const baseUrl = 'https://api.themoviedb.org/3';
        const imageBaseUrl = 'https://image.tmdb.org/t/p/w500';
        const highResImageBaseUrl = 'https://image.tmdb.org/t/p/w780';
        
        const moviesCarousel = document.getElementById('moviesCarousel');
        const movieDetails = document.getElementById('movieDetails');
        const detailsContent = document.getElementById('detailsContent');
        const searchInput = document.getElementById('searchInput');
        const hero = document.querySelector('.hero');
        const carouselSection = document.querySelector('.carousel-section');
        
        // Load latest movies on page load
        window.onload = () => {
            fetchMovies(`${baseUrl}/movie/now_playing?api_key=${apiKey}`);
        };
        
        // Search functionality
        searchInput.addEventListener('input', (e) => {
            const query = e.target.value.trim();
            if (query) {
                fetchMovies(`${baseUrl}/search/movie?api_key=${apiKey}&query=${encodeURIComponent(query)}`);
            } else {
                fetchMovies(`${baseUrl}/movie/now_playing?api_key=${apiKey}`);
            }
        });
        
        // Fetch and display movies
        function fetchMovies(url) {
            moviesCarousel.innerHTML = '<div class="loading"><div class="spinner"></div><p>Loading movies...</p></div>';
            fetch(url)
                .then(response => {
                    if (!response.ok) throw new Error('API request failed');
                    return response.json();
                })
                .then(data => {
                    if (data.results && data.results.length > 0) {
                        displayMovies(data.results);
                    } else {
                        moviesCarousel.innerHTML = '<div class="error">No movies found. Try a different search.</div>';
                    }
                })
                .catch(error => {
                    console.error('Error fetching movies:', error);
                    moviesCarousel.innerHTML = '<div class="error">Failed to load movies. Check your API key or internet connection.</div>';
                });
        }
        
        function displayMovies(movies) {
            moviesCarousel.innerHTML = '';
            movies.forEach(movie => {
                if (!movie.poster_path) return; // Skip movies without posters
                const movieCard = document.createElement('div');
                movieCard.className = 'movie-card';
                movieCard.onclick = () => showMovieDetails(movie.id);
                movieCard.innerHTML = `
                    <img src="${imageBaseUrl}${movie.poster_path}" alt="Poster for ${movie.title}" loading="lazy">
                    <div class="content">
                        <h3>${movie.title}</h3>
                        <p>${movie.release_date ? movie.release_date.split('-')[0] : 'N/A'}</p>
                    </div>
                `;
                moviesCarousel.appendChild(movieCard);
            });
        }
        
        // Show movie details
        function showMovieDetails(movieId) {
            detailsContent.innerHTML = '<div class="loading"><div class="spinner"></div><p>Loading details...</p></div>';
            Promise.all([
                fetch(`${baseUrl}/movie/${movieId}?api_key=${apiKey}`).then(r => r.json()),
                fetch(`${baseUrl}/movie/${movieId}/credits?api_key=${apiKey}`).then(r => r.json()),
                fetch(`${baseUrl}/movie/${movieId}/similar?api_key=${apiKey}`).then(r => r.json())
            ])
            .then(([movie, credits, similar]) => {
                if (!movie.poster_path) movie.poster_path = ''; // Fallback
                detailsContent.innerHTML = `
                    <div class="poster">
                        <img src="${movie.poster_path ? highResImageBaseUrl + movie.poster_path : 'https://via.placeholder.com/300x450/cccccc/000000?text=No+Poster'}" alt="Poster for ${movie.title}">
                    </div>
                    <div class="info">
                        <h1>${movie.title}</h1>
                        <div class="rating">
                            <i class="fas fa-star"></i>
                            <span>${movie.vote_average ? movie.vote_average.toFixed(1) : 'N/A'}/10</span>
                        </div>
                        <p><strong>Release Date:</strong> ${movie.release_date || 'N/A'}</p>
                        <p><strong>Genres:</strong> ${movie.genres ? movie.genres.map(g => g.name).join(', ') : 'N/A'}</p>
                        <div class="overview">
                            <h3>Overview</h3>
                            <p>${movie.overview || 'No overview available.'}</p>
                        </div>
                        <div class="cast-crew">
                            <h3>Cast & Crew</h3>
                            <div class="cast-list">
                                ${credits.cast && credits.cast.length > 0 ? credits.cast.slice(0, 10).map(person => `
                                    <div class="cast-member">
                                        <img src="${person.profile_path ? imageBaseUrl + person.profile_path : 'https://via.placeholder.com/100x100/cccccc/000
