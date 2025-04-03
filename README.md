1. Place the docker-compose.yml File in a Folder
	1.	Create or choose a folder on your machine (e.g., my-demo-project/).
	2.	Copy the docker-compose.yml contents into a file named docker-compose.yml in that folder.
	3.	(Optional) If your Compose file references environment variables, also place a .env file there with the required variables.

Your folder structure might look like this:
my-demo-project
 ├─ docker-compose.yml
 └─ .env

 2. Open a Terminal or Command Prompt

Open your favorite terminal/command prompt on your machine:
	•	Windows: You can use PowerShell, Command Prompt, or WSL (Windows Subsystem for Linux).
	•	macOS/Linux: Use the default Terminal.

Ensure your current directory is the same folder containing your docker-compose.yml. For example:
cd path/to/my-demo-project

3. (Optional) Decide Which Profile to Use

Your docker-compose file contains multiple Docker Compose “profiles” (e.g., cpu, gpu-nvidia, gpu-amd). Profiles allow you to selectively start only certain services.
	•	CPU profile: will start CPU-based services (like ollama-cpu).
	•	GPU profiles: will start GPU-based services (e.g., ollama-gpu or ollama-gpu-amd).

If you don’t use any --profile <name> flags, Docker will by default start only the services that do not have a profiles: key, or those that have an empty array for profiles:.

4. Run the Compose File via Docker CLI

A. If You Want the CPU-based Stack
docker compose --profile cpu up -d

B. If You Want the NVIDIA GPU-based Stack
docker compose --profile gpu-nvidia up -d

C. If You Want the AMD GPU-based Stack
docker compose --profile gpu-amd up -d

Tip: -d (detached) runs containers in the background. Omit -d to see logs directly in your terminal.

5. Check Containers in Docker Desktop
	1.	Open Docker Desktop.
	2.	Click on the Containers / Apps tab (or “Containers” on the left sidebar in newer versions).
	3.	You will see your “stack” (the name is usually derived from the folder name if you haven’t set a project_name in Docker Compose).
	4.	Expand that stack to see all the running containers:
	•	postgres
	•	n8n-import, n8n
	•	qdrant
	•	ollama-cpu or ollama-gpu (depending on your profile)
	•	ollama-pull-llama-cpu / -gpu (also profile dependent)
	•	open-webui (if included in the profile)

⸻

6. Verify Everything Is Running
	•	In Docker Desktop, each container should have a green “Running” indicator if everything is healthy.
	•	You can also run:
docker compose ps

This command lists container states (e.g., Up, Exited, Healthy).

7. Access the Services

Depending on your configuration, you might have mapped ports like:
	•	n8n on localhost:5678
	•	Open WebUI on localhost:3000
	•	Ollama on localhost:11434
	•	Qdrant on localhost:6333

Visit these in your browser or call them via an API client to verify they respond as expected.

⸻

8. Stopping/Removing the Containers

When you’re done using the stack, you can stop the containers with:
docker compose down

To remove the containers (but keep named volumes), add the -v flag (careful, this removes anonymous volumes). If you want to keep your data, do not use -v. Named volumes (defined in your docker-compose.yml) are retained by default unless you specify --volumes.

Additional Tips
	•	Environment Variables: If you have variables like POSTGRES_USER, POSTGRES_PASSWORD, or anything else, ensure they’re defined in your .env or in your shell environment.
	•	Logs: To view logs for a single container (e.g., n8n), use:
 docker compose logs -f n8n

 	•	Updating Services: If you change the docker-compose.yml or .env, run:
  docker compose up -d

  again to apply changes.

	•	Profile Combinations: If you want to combine profiles (e.g., CPU + GPU for some reason), you can pass multiple --profile flags:
 docker compose --profile cpu --profile gpu-nvidia up -d

 However, be sure your services can coexist without conflicting ports or container names.



