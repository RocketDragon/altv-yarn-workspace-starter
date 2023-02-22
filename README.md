# altv-yarn-workspace-starter

A baseline structure to start using yarn workspaces to build resources for a server.

## Prerequisites

- `altv-server.exe` & `js-module` / `js-bytecode-module` ([Download (altv.mp)](https://altv.mp/#/downloads))
  > Adjust resource manifests as neccesary.
- `npm i -g yarn`

## Creating a new resource within the workspace

1. Copy the `template-js` / `template-ts` folder.
2. Rename the folder, and give it the same name in the `package.json`.
3. Run `start.sh` to start the server.

### Edge-cases

- When starting the server, run `yarn workspaces run build`.
  > If this does not work, the `npm` alternative has been provided as a comment.
- If `yarn` is the primary motivator to use this template, all workspace *resources* must be given a **name**, **version** and `build` **script** (even if the script is `exit 0`, as demonstrated in the `common` resource).
- Use `yarn add -W <pkg>` when adding packages to the root manifest.

## Setting up with TypeScript

1. Run `yarn add -D typescript` adding it to the base workspace manifest.
2. In any resources that are to use typescript, run

## Additional notes

While this does server to be the outline of a server assembly, it does not come anywhere close to supporting 'hot loading' - which [altv-esbuild](https://github.com/xxshady/altv-esbuild) can already achieve. This starter structure attempts to be as close to the intented production functionality of AltV's framework without listening for changes or restarting the server.

> Internal resources will not transfer type data (at least, not for now).
> 

### Command ecosystem equivelents

*`npm run <script>` commands can have `--if-present` added at the end to safely exit without a fuss, `yarn` does not have this until its v2 release ([Maintainer comment on yarnpkg/yarn#6894](https://github.com/yarnpkg/yarn/issues/6894#issuecomment-646579736)).*

> Decisions on using `npm` or `yarn` does not impede core functionality if it is only targeting scripts.

| Action | `npm` | `yarn` |
| ------ | ----- | ------ |
| Install / refresh for all | `npm i --ws` | `yarn` |
| Install for one | `npm i <pkg> -w=<name>` | `yarn workspace <name> add <pkg>` |
| Remove for one | `npm remove <pkg> -w=<name>` | `yarn workspace <name> remove <pkg>` |
| Install / Remove for root | `npm install/remove <pkg>` | `yarn add/remove <pkg> -W` |
| Run for all | `npm run <script> --ws [--if-present]` | `yarn workspaces run <script>` |
| Run for one | `npm run <script> -w=<name> [--if-present]` | `yarn workspace <name> run <script>` |
| Workspace info | *Unknown* | `yarn workspaces info` |
| Run binary for all | `npm exec --ws -c <cmd> [args...]` / `npx ...` | `yarn workspaces run <cmd> [args...]` |
| Run binary for one | `npm exec -w=<name> -c <cmd> [args...]` / `npx ...` | `yarn workspace <name> run <cmd> [args...]` |

> `~/node_module/.bin/*` scripts can also be run from `yarn workspaces run <bin>` - which will run from the bin folder within the workspace.

### Dependency management

Depencendies of any kind can be specified in the root manifest, or in any workspace manifest. All packages will be loaded into the root `node_modules` folder for universal management (while using `yarn`).

*Once a package manager is chosen (`npm`, `yarn`), it is important to remain with that package manager as only it can best read its own manifests - 3rd party manifests to NPMs native implementation with node can read the package-lock file created by its cli, but are usually much slower at converting to their own format.*

> Manifests have been setup with the basic dependencies to ensure TypeScript and Intellisense are satisfied, should you end up using them.

### Build tasks

> For TypeScript, Babel, etc. compilers

If you are having trouble with files not being removed on a build task, add the `rimraf` cli package and prepend each build script with `rimraf ./dist &&` (or whatever the build target is set to) or prepend `npx` *on top of that* to avoid adding it as an immediate dependency.
