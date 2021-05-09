# gitHubProfilesApp
Allows for GitHub user search with developed UI using HTML, CSS, and JS - also utilizes an API.

This project creates a user search for GitHub accounts which pulls up members, some of their main works and bio's.

Building this projects has helped me to better learn and practice the following:
1) Working with API's
2) async/await
3) DOM manipulation
4) Developing & implementing logic

A general walkthrough of the JavaScript code is given below.

First, construct the getUser() function to fetch data from the API and use that data to create the user's card.
```JavaScript
async function getUser(username){
    const resp = await fetch(APIURL + username);
    const respData = await resp.json();

    createUserCard(respData);
    getRepos(username);
}
```

Construct the createUserCard() function.
```JavaScript
function createUserCard(user){
    const cardHTML = `
    <div class="card">
        <div class="img-container">
        <img class="avatar" src="${user.avatar_url}" alt="${user.name}">
        </div>  
        <div class="user-info">
            <h2>${user.login}</h2>
            <p>${user.bio}</p>

            <ul class="info">
                <li><svg class="color-1" xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z" />
              </svg>followers&nbsp;&nbsp;${user.followers}</li>
                <li><svg class="color-2" xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                <path fill-rule="evenodd" d="M3.172 5.172a4 4 0 015.656 0L10 6.343l1.172-1.171a4 4 0 115.656 5.656L10 17.657l-6.828-6.829a4 4 0 010-5.656z" clip-rule="evenodd" />
              </svg>following&nbsp;&nbsp;${user.following}</li>
                <li><svg class="color-3" xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                <path fill-rule="evenodd" d="M5 2a1 1 0 011 1v1h1a1 1 0 010 2H6v1a1 1 0 01-2 0V6H3a1 1 0 010-2h1V3a1 1 0 011-1zm0 10a1 1 0 011 1v1h1a1 1 0 110 2H6v1a1 1 0 11-2 0v-1H3a1 1 0 110-2h1v-1a1 1 0 011-1zM12 2a1 1 0 01.967.744L14.146 7.2 17.5 9.134a1 1 0 010 1.732l-3.354 1.935-1.18 4.455a1 1 0 01-1.933 0L9.854 12.8 6.5 10.866a1 1 0 010-1.732l3.354-1.935 1.18-4.455A1 1 0 0112 2z" clip-rule="evenodd" />
              </svg>public repos&nbsp;&nbsp;${user.public_repos}</li>
            </ul>
            <h4>Repos:</h4>
            <div id="repos">
            </div>
        </div> 
    </div> 
    `;
   

    main.innerHTML = cardHTML;
}
```

Connect the form submit to query user searches.
```JavaScript
form.addEventListener('submit', (e) => {
    e.preventDefault();

    const user = search.value;

    if(user) {
        getUser(user);
        search.value = '';
    }
})
```

Construct getRepos() function to fetch the public repositories of the queried user.
```JavaScript
async function getRepos(username) {
    const resp = await fetch(APIURL + username + "/repos");
    const respData = await resp.json();

    addReposToCard(respData); 
}
```

Construct addReposToCard() function to add the fetched repositories into a list underneath the user profile.
```JavaScript
function addReposToCard(repos){
    const reposEl = document.getElementById('repos');
  
    repos.sort((a,b) => b.stargazers_count - a.stargazers_count).slice(0,30).forEach(repo => {
        const repoEl = document.createElement('a');
        repoEl.classList.add('repo');

        repoEl.href = repo.html_url;
        repoEl.target = "_blank";
        repoEl.innerText = repo.name;

        reposEl.appendChild(repoEl);
    })
}
```

***End walkthrough
