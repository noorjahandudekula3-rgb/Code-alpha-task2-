
const BACKEND_URL = 'http://localhost:3000';

// 1. Load all posts when the page opens
async function loadPosts() {
    const response = await fetch(`${BACKEND_URL}/posts`);
    const posts = await response.json();
    
    const feed = document.getElementById('feed');
    feed.innerHTML = ''; // Clear old posts

    posts.forEach(post => {
        feed.innerHTML += `
            <div class="post">
                <strong>@${post.username}</strong>
                <p>${post.content}</p>
                <button onclick="likePost('${post._id}')">❤️ ${post.likesCount}</button>
            </div>
        `;
    });
}

// 2. Send a new post to the backend
async function submitPost() {
    const text = document.getElementById('postText').value;
    
    await fetch(`${BACKEND_URL}/posts`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ content: text, username: 'GuestUser' }) // simple default user for now
    });

    document.getElementById('postText').value = ''; // Clear input
    loadPosts(); // Refresh the feed
}

// 3. Like a post
async function likePost(postId) {
    await fetch(`${BACKEND_URL}/posts/${postId}/like`, { method: 'POST' });
    loadPosts(); // Refresh to see updated numbers
}

// Run this automatically when the page loads
loadPosts();
