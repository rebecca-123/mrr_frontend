<html>
<style>
        button {
        padding: 10px 20px;
        background-color: #161666;
        color: white;
        border: none;
        border-radius: 5px;
        font-size: 16px;
        cursor: pointer;
        margin: 8px 5px;    }
        input[type="text"] {
        padding: 10px;
        border: 2px solid blue;
        border-radius: 5px;
        font-size: 16px;
        width: 200px;
        margin: 8px 5px;
            }
                .input-container {
        display: flex;
        justify-content: center;
        margin-bottom: 20px;
    }
        #table-container th {
        color: white;
    }
    #table-container td {
    color: white;
}
        h1, h2, h3{
                color: #fff;
                position: center;
            }
       body {
            background-color: #01060d;
        }
        #addUserLink {
        padding: 10px 20px;
        background-color: #161666;
        color: white;
        border: none;
        border-radius: 5px;
        font-size: 16px;
        cursor: pointer;
        margin: 8px 5px;    }
        </style>
    <body>
        <h1 class="text-center m-5">Team Members</h1>
                <div class="input-container">
                     <p><a id="addUserLink">ADD A MEMBER TO YOUR TEAM</a></p>
        <div class="table-responsive mx-5">
            <table id="table-container" class="table table-hover table-bordered border-secondary mb-5">
                <thead>
                    <tr>
                        <th scope="col">Student Name </th>
                        <th scope="col">Github ID</th>
                        <th scope="col">Blog</th>
                    </tr>
                </thead>
                <tbody class="table-group-divider" id="users">
                </tbody>
            </table>
        </div>
        <script>
            const urlParams = new URLSearchParams(window.location.search);
            const teamId = urlParams.get('id');
            var a = document.getElementById('addUserLink');
            a.href = `https://rebecca-123.github.io/mrr_frontend/addUser?id=${teamId}`
            // prepare fetch urls
            const team_url = `https://mrr.rebeccaaa.tk/api/team/${teamId}`;
            const get_url = team_url + "/";
            const teamContainer = document.getElementById("users");
            // prepare fetch GET options
            const options = {
                method: 'GET', // *GET, POST, PUT, DELETE, etc.
                // mode: 'cors', // no-cors, *cors, same-origin
                cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
                // credentials: 'same-origin', // include, same-origin, omit
                headers: {
                'Content-Type': 'application/json'
                // 'Content-Type': 'application/x-www-form-urlencoded',
                },
            };
            // fetch the API
            fetch(get_url, options)
                // response is a RESTful "promise" on any successful fetch
                .then(response => {
                // check for response errors
                if (response.status !== 200) {
                    error('GET API response failure: ' + response.status);
                    return;
                }
                // valid response will have JSON data
                response.json().then(data => {
                    const users = data.names;
                    for (const user of users) {
                        console.log(user);
                        // columns
                        const tr = document.createElement("tr");
                        const name = document.createElement("td");
                        const ghid = document.createElement("td");
                        const blog = document.createElement("td");
                        // url containers
                        // accessing JSON values
                        name.innerHTML = user.name;
                        ghid.innerHTML = user.githubId;
                        blog.innerHTML = `<a target="_blank" href="http://${user.blog}">Blog for ${user.name}</a>`;
                        tr.appendChild(name);
                        tr.appendChild(ghid);
                        tr.appendChild(blog);
                        // add row to table
                        teamContainer.appendChild(tr);
                    }
                })
            })
            // catch fetch errors (ie Nginx ACCESS to server blocked)
            .catch(err => {
                error(err + " " + get_url);
            });
            // Something went wrong with actions or responses
            function error(err) {
                // log as Error in console
                console.error(err);
                // append error to resultContainer
                const tr = document.createElement("tr");
                const td = document.createElement("td");
                td.innerHTML = err;
                tr.appendChild(td);
                teamContainer.appendChild(tr);
            }
            function addUser() {
                const urlParams = new URLSearchParams(window.location.search);
                const teamId = urlParams.get('id');
                const name = document.getElementById('name').value;
                const githubId = document.getElementById('github-id').value;
                const profileLink = document.getElementById('profile-link').value;
                const user = {
                    "name": name,
                    "githubId": githubId,
                    "profileLink": profileLink
                };
                const endpoint = `https://mrr.rebeccaaa.tk/api/team/addMember/${teamId}`;
                const options = {
                    method: 'POST',
                    mode: 'cors',
                    cache: 'no-cache',
                    credentials: 'include',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify(data)
                }
                fetch(endpoint, options)
                    .then(response => response.json())
                    .then(data => {
                    console.log('User creation response:', data);
                    // Handle the response from the server
                    // You can perform further actions based on the response
                    })
            }        </script>
