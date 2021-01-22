UPDATE: the cause was actually eslint's import/no-unresolved rule. It doesn't know how to read the baseUrl in in this project setup and it doesn't show the squiglies until after you open up your .eslintrc file so it's a little tricky to track down. Just disabled that rule since typescript checks on its own.

## Repro repo for a possible vscode bug

This repo demos an apparent bug in vscode's intellisense when using typescript with monorepos and opening your projects from the root of the monorepo.

If you set the `baseUrl` in your `tsconfig.json` file to configure it to allow non-relative imports starting at your `src` folder for your subproject then intellisense only works if you open up the subproject directly. It doesn't work if you open up the monorepo root in vscode.

The use case here is yarn monorepos. The problem is probably unrelated to monorepos in general but structuring your project this way and opening it from a parent directory mostly only makes sense if you're using something like a monorepo.

### Steps

To reproduce

- check out this repo
- run `yarn install`. You do need to use yarn (tested with v1.22.5) since this is a yarn monorepo.
- open the base of the repo in vscode
- open the `projects/app/src/index.ts` file
- observe the red squiglies under the import. The import should work because of the `baseUrl` in the `tsconfig.`.
- open vscode again but rooted at `projects/app` instead of the repo root.
- observe that the red squiglies are gone.

Normal builds with `tsc` always work and can be run with `yarn build` from either the root or the subproject.
