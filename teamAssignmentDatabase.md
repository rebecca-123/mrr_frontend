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
        #addAssignmentLink {
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
        <h1 class="text-center m-5">Team Assignments</h1>
        <div class="table-responsive mx-5">
            <table id="table-container" class="table table-hover table-bordered border-secondary mb-5">
                <thead>
                    <tr>
                        <th scope="col">Assignment </th>
                        <th scope="col">Score</th>
                        <th scope="col">Ticket</th>
                        <th scope="col">Comments</th>
                    </tr>
                </thead>
                <tbody class="table-group-divider" id="users">
                </tbody>
            </table>
        </div>
            <div class="input-container">
                <p><a id="addAssignmentLink">Add Assignment</a></p>
        <script>
            const urlParams = new URLSearchParams(window.location.search);
            const teamId = urlParams.get('id');
            var a = document.getElementById('addAssignmentLink');
            a.href = `https://rebecca-123.github.io/mrr_frontend/addAssignment?id=${teamId}`
            // prepare fetch urls
            const team_url = `https://mrr.rebeccaaa.tk/database/reviews/${teamId}`;
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
                    const assignments = data;
                    for (const assign of assignments) {
                        console.log(assign);
                        // columns
                        const tr = document.createElement("tr");
                        const assignment = document.createElement("td");
                        const score = document.createElement("td");
                        const ticket = document.createElement("td");
                        const comments = document.createElement("td");
                        // url containers
                        // accessing JSON values
                        assignment.innerHTML = assign.assignment;
                        score.innerHTML = assign.score;
                        ticket.innerHTML = assign.ticket;
                        comments.innerHTML = assign.comments;
                        tr.appendChild(assignment);
                        tr.appendChild(score);
                        tr.appendChild(ticket);
                        tr.appendChild(comments);
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
            function addAssignment() {
                const urlParams = new URLSearchParams(window.location.search);
                const teamId = urlParams.get('id');
                const assignment = document.getElementById('assignment').value;
                const score = document.getElementById('score').value;
                const ticket = document.getElementById('ticket').value;
                const comments = document.getElementById('comments').value;
                const user = {
                    assignment: assignment,
                    score: score,
                    ticket: ticket,
                    comments: comments,
                };
                const options = {
                    method: 'POST',
                    mode: 'cors',
                    cache: 'no-cache',
                    credentials: 'include',
                    headers: {
                    'Content-Type': 'application/json'
                    },
                    body: JSON.stringify(user), // convert to JSON
                };
                const endpoint = `https://mrr.rebeccaaa.tk/database/addreview/${teamId}`;
                    fetch(endpoint, options)
                    .then(response => response.json())
                    .then(data => {
                    console.log('User creation response:', data);
                    // Handle the response from the server
                    // You can perform further actions based on the response
                    })
            }        
            </script>