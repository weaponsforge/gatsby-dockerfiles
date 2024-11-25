## gatsby-dockerfiles

A collection of development Dockerfiles for running various [Gatsby CLI](https://www.gatsbyjs.com/docs/reference/gatsby-cli/) versions in containers as an alternative to global installation `"npm install -g gatsby-cli"` on a host machine.

This repository contains references to Gatsby CLI Docker recipes and notes about which Node version(s) are compatible with each CLI version for quick look-ups and inspection of existing Gatsby themes and starters.

### 📦 Gatsby Dependencies

Here are some known Gatsby CLI versions and their compatible Node versions.

| Gatsby CLI | Gatsby CLI<br>latest | Gatsby CLI<br>Version | Compatible Node<br>Versions | Docker Review |
| :---: | :---: | :---: | --- | --- |
| v5 | - | v5.14.0 | v18.x (LTS), v20.x  | Build success |
| v4 | [latest-v4](https://www.npmjs.com/package/gatsby-cli/v/latest-v4) | v4.25.0 | 14.x, 16.x, or 18.x | Build success |
| v3 | [latest-v3](https://www.npmjs.com/package/gatsby-cli/v/latest-v3) | v3.15.0 | 12.x, 14.x, or 16.x | Build success |
| v2 | [latest-v2](https://www.npmjs.com/package/gatsby-cli/v/latest-v2) | v2.19.3 | v12.x | Needs native libs for building the sharp image lib; often missing built libs for the webp format |

## 🛠️ Initialization

1. [Create a new Gatsby app](https://www.gatsbyjs.com/docs/tutorial/getting-started/), or use one of the existing [templates](https://www.builtatlightspeed.com/category/gatsby) or [themes](https://jamstackthemes.dev/theme/#ssg=gatsby).

2. Take note of the app's required Gatsby CLI and Node version, which can be deduced from its `"package.json"`, `"package-lock.json"`, or `"yarn.lock"` files.
   > **TIP:** Use the recommended Node version in the host machine when using **only Node** and the **global Gatsby CLI installation** for development. For example, to initialize a new Gatsby app using the **Gatsby CLI v4**, switch to Node 16.14.2 - `"nvm use 16.14.2."`

3. Use the Dockerfiles inside the **/v2**, **/v3**, **/v4**, or **/v5** directories for reference to build a target Gatsby CLI Docker image.

   | Folder | Description |
   | :---: | --- |
   | /v5 | Dockerfiles and compose files for Gatsby CLI v5 |
   | /v4 | Dockerfiles and compose files for Gatsby CLI v4 |
   | /v3 | Dockerfiles and compose files for Gatsby CLI v3 |
   | /v2 | Dockerfiles and compose files for Gatsby CLI v2 |

4. Build a Gatsby CLI image. For example, to build the **Gatsby CLI v4** image, navigate first to the **/v4** directory, then run:
   ```bash
   docker compose -f docker-compose.v4-16x.yml build
   ```

## 🚀 Usage

Run the Gatsby CLI in Docker. For example, to run the **Gatsby CLI v4** [API commands](https://www.gatsbyjs.com/docs/reference/gatsby-cli/#api-commands) (using PowerShell):

- **Gatsby CLI available API commands:**
   ```bash
   docker run -it --rm -v ${pwd}:/app weaponsforge/gatsby-cli-v4 gatsby <API_COMMAND>
   ```

- **Run the `"gatsby develop"` command (inside a Gatsby app directory):**
   ```bash
   docker run -it --rm -v ${pwd}:/app -w /app -v /app/node_modules -p 8000:8000 weaponsforge/gatsby-cli-v4-16x sh -c "npm install && gatsby develop -H 0.0.0.0 --verbose"
   ```

- **Create a `Docker compose` YML file to run an existing Gatsby app** (an alternate option to the previous step):<br>

   ```yaml
   services:
     gatsby-app:
       image: weaponsforge/gatsby-cli-v4-16x
       working_dir: /app
       volumes:
         - .:/app
         - /app/node_modules
       ports:
         - "8000:8000"
       command: sh -c "npm install && gatsby develop --host 0.0.0.0"
   ```
<br>

> [!TIP]
> Run the dockerized Gatsby CLI API commands using different Docker volume mount combinations to achieve the best results.

@weaponsforge<br>
20241125
