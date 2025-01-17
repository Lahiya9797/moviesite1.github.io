<!DOCTYPE html>
<html lang="en">   
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Movie and TV Series Tracker</title>
    <style>
        
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background: linear-gradient(to right, #4facfe, #00f2fe);
            color: #333;
        }
        
        header {
            background-color: #1f1f1f;
            color: #ffffff;
            padding: 15px 20px;
            text-align: center;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
        }

        main {
            padding: 20px;
            max-width: 800px;
            margin: 20px auto;
            background-color: #ffffff;
            border-radius: 8px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
        }

        .form-section {
            background: #f4f9ff;
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            margin-bottom: 20px;
            border: 2px solid #4facfe;
        }

        .form-section h2 {
            margin-top: 0;
            text-align: center;
            color: #4facfe;
            font-size: 24px;
            font-weight: bold;
        }

        .form-group {
            margin-bottom: 15px;
        }

        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
            color: #333;
        }

        .form-group input, .form-group textarea, .form-group select {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 6px;
            font-size: 14px;
            transition: border-color 0.3s, box-shadow 0.3s;
        }

        .form-group input:focus, .form-group textarea:focus, .form-group select:focus {
            border-color: #4facfe;
            box-shadow: 0 0 5px rgba(79, 172, 254, 0.5);
            outline: none;
        }

        .form-group button {
            width: 100%;
            padding: 12px;
            background-color: #4facfe;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            transition: background-color 0.3s;
        }

        .form-group button:hover {
            background-color: #3b8efc;
        }

        .link-button {
            display: inline-block;
            margin: 15px 0;
            padding: 10px 20px;
            background-color: #4facfe;
            color: white;
            text-decoration: none;
            border-radius: 8px;
            font-size: 14px;
            font-weight: bold;
            transition: background-color 0.3s;
        }

        .link-button:hover {
            background-color: #3b8efc;
        }

        .movie-list {
            background: #f9f9f9;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }

        .movie-item {
            border-bottom: 1px solid #ccc;
            padding: 10px 0;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .movie-item:last-child {
            border-bottom: none;
        }

        .movie-item strong {
            color: #333;
        }

        .action-buttons button {
            margin-left: 10px;
            padding: 6px 10px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            transition: background-color 0.3s;
        }

        .action-buttons .delete-button {
            background-color: #f44336;
            color: white;
        }

        .action-buttons .delete-button:hover {
            background-color: #e53935;
        }

        .action-buttons .sort-button {
            background-color: #4caf50;
            color: white;
        }

        .action-buttons .sort-button:hover {
            background-color: #43a047;
        }
    </style>
</head>
<body>
    <header>
        <h1>My Movie and TV Series Tracker</h1>
    </header>
    <main>
        <div id="addPage">
            <div class="form-section">
                <h2>Add a New Movie or TV Series</h2>
                <div class="form-group">
                    <label for="title">Title</label>
                    <input type="text" id="title" placeholder="Enter title">
                </div>
                <div class="form-group">
                    <label for="type">Type</label>
                    <select id="type">
                        <option value="movie">Movie</option>
                        <option value="tv">TV Series</option>
                    </select>
                </div>
                <div class="form-group">
                    <label for="notes">Notes</label>
                    <textarea id="notes" placeholder="Add any notes"></textarea>
                </div>
                <div class="form-group">
                    <button onclick="addEntry()">Add to List</button>
                </div>
            </div>
            <a href="#" class="link-button" onclick="showPage('listPage')">View My List</a>
        </div>

        <div id="listPage" style="display: none;">
            <div class="movie-list">
                <h2>Watched Movies and TV Series</h2>
                <div class="action-buttons">
                    <button class="sort-button" onclick="sortList()">Sort by Title</button>
                </div>
                <div id="movieContainer"></div>
            </div>
            <a href="#" class="link-button" onclick="showPage('addPage')">Add More</a>
        </div>
    </main>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const storedMovies = JSON.parse(localStorage.getItem('movies')) || [];
            storedMovies.forEach(movie => {
                displayEntry(movie.title, movie.type, movie.notes);
            });
        });

        function addEntry() {
            const title = document.getElementById('title').value;
            const type = document.getElementById('type').value;
            const notes = document.getElementById('notes').value;

            if (!title) {
                alert('Please enter a title');
                return;
            }

            const movie = { title, type, notes };

            const movies = JSON.parse(localStorage.getItem('movies')) || [];
            movies.push(movie);
            localStorage.setItem('movies', JSON.stringify(movies));

            displayEntry(title, type, notes);

            document.getElementById('title').value = '';
            document.getElementById('type').value = 'movie';
            document.getElementById('notes').value = '';
        }

        function displayEntry(title, type, notes) {
            const movieContainer = document.getElementById('movieContainer');

            const movieItem = document.createElement('div');
            movieItem.className = 'movie-item';

            movieItem.innerHTML = `
                <div>
                    <strong>Title:</strong> ${title} <br>
                    <strong>Type:</strong> ${type === 'movie' ? 'Movie' : 'TV Series'} <br>
                    <strong>Notes:</strong> ${notes || 'N/A'}
                </div>
                <div class="action-buttons">
                    <button class="delete-button" onclick="deleteEntry('${title}')">Delete</button>
                </div>
            `;

            movieContainer.appendChild(movieItem);
        }

        function deleteEntry(title) {
            const movies = JSON.parse(localStorage.getItem('movies')) || [];
            const updatedMovies = movies.filter(movie => movie.title !== title);
            localStorage.setItem('movies', JSON.stringify(updatedMovies));

            const movieContainer = document.getElementById('movieContainer');
            movieContainer.innerHTML = '';
            updatedMovies.forEach(movie => displayEntry(movie.title, movie.type, movie.notes));
        }

        function sortList() {
            const movies = JSON.parse(localStorage.getItem('movies')) || [];
            movies.sort((a, b) => a.title.localeCompare(b.title));
            localStorage.setItem('movies', JSON.stringify(movies));

            const movieContainer = document.getElementById('movieContainer');
            movieContainer.innerHTML = '';
            movies.forEach(movie => displayEntry(movie.title, movie.type, movie.notes));
        }

        function showPage(pageId) {
            document.getElementById('addPage').style.display = pageId === 'addPage' ? 'block' : 'none';
            document.getElementById('listPage').style.display = pageId === 'listPage' ? 'block' : 'none';
        }
    </script>
