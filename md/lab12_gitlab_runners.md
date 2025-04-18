
#### Run GitLab Runner in a container

We can run github runners locally. In this lab, we will register gitlab-runner on `gitlab.com`.


**NOTE:** Make sure to open new terminal and connect with your remote VM before running docker commands below:

`ssh fenago@YOUR_VM_DNS_NAME`

**Username/Password:** Will be provided by Instructor.

![](./images/19.png)


In this lab, you can use a configuration container to mount your custom data volume.

Create the Docker volume:

```
docker volume create gitlab-runner-config
```

Start the GitLab Runner container using the volume we just created:

```
docker run -d --name gitlab-runner --restart always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v gitlab-runner-config:/etc/gitlab-runner \
    gitlab/gitlab-runner:latest
```


#### Group Runner Setup

1. On the top bar, select `Main menu` > `Groups` and find your group.
![](./images/23.png)

  **Note:** If you don't have one, click `New Group` to create new one.

2. On the left sidebar, select `Build` > `Runners`.
![](./images/24.png)

3. In the upper-right corner, select `New group runner`.
![](./images/25.png)

4. Check **Run untagged jobs**, add **Runner description** and create `New Runner`.
![](./images/25_1.png)

5. **Important:** Make sure to copy the token from the UI for next step (You will get different new token).

![](./images/25_2.png)


### Register a Runner

1. To register a runner using a Docker container:

```
docker run --rm -it -v gitlab-runner-config:/etc/gitlab-runner gitlab/gitlab-runner:latest register
```

2. Enter your GitLab instance URL (also known as the gitlab-ci coordinator URL):
    `https://gitlab.com/`
3. Enter the token you obtained to register the runner.
4. Enter a name for the runner
5. Provide the runner executor: enter **docker**.
6. If you entered docker as your executor, you are asked for the default image to be used for projects that do not define one in .gitlab-ci.yml: enter **ruby:2.7**
![](./images/27.png)

7. Open/Reload the runners webpage, you should see one runner available.
![](./images/28.png)


### Using Gitlab Runner in CI/CD

1. **Important!** Make sure to create new project inside the group so that you can use the runner:
![](./images/29.png)

2. Disable instance runners for all the projects that you create. Otherwise, gitlab will fail your pipeline and ask for account verification.

3. On the left sidebar, select `Settings` > `CI/CD`.
![](./images/30.png)

4. Expand `Runners` and disable instance runners for this project.
![](./images/31.png)

5. Scroll to `Group runners` and confirm one runner is available.
![](./images/32.png)


**Important!** You will need to disable shared runner for all gitlab projects.
