# grunt-git-selective-deploy

> Create a new branch on-the-fly from your git repo and selectively deploy files to any remote branch.

## Getting Started

This plugin requires Grunt `~0.4.1`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-git-selective-deploy --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-git-selective-deploy');
```

## The "git_deploy" task

### Overview

The way this task works is it creates an empty git repository in the `src` directory you specify, creates an orphan branch and commits all files from that directory to it.
Then it pushes the branch to the configured remote repository. **Be careful as this destroys the history of the remote branch.**. It also creates a `.gitignore` containing
the files/folders you specify, so you can selectively push the filelist.

In your project's `Gruntfile.js` file, add a section named `git_deploy` to the data object passed into `grunt.initConfig()`.

```js
grunt.initConfig({
  git_deploy: {
    your_target: {
      options: {
        url: 'git@github.com:example/repo.git'
      },
      src: 'directory/to/deploy' // you may use . for the current directory that Gruntfile.js is
    },
  },
})
```

and execute it from the command line

```bash
grunt git_deploy your_target
```

### Options

#### options.url

Type: `String`

The URL to a remote git repository. This option is required.

#### options.localBranch

Type: `String`
Default value: `'master'`

The local branch to create a new branch from.

#### options.remoteBranch

Type: `String`
Default value: `'master'`

The remote branch to push to.

#### options.message

Type: `String`
Default value: `'git deploy'`

Commit message. You may use grunt template language `<% %>` in this message, like to pass in a version for example, automatically.

```js
pkg: grunt.file.readJSON('package.json'),
//...
    options:{
        message: 'commit v<%= pkg.version %>'
    }
```

#### options.quiet

Type: `Boolean`
Default value: `true`

Set to false to see git results.

#### options.ignore

Type: `Array`
Default value:

```js
['Gruntfile.js','node_modules','nbproject','README.md','test','**/*.scss','**/*.sass','.sass-cache','.idea','.DS_Store','config.rb']
```

Ignore files on-the-fly to avoid development files to be commited to the server. This will fill new git repo root `.gitignore` with
all the found files and folders.

#### options.ignoreAppend

Type: `Boolean`
Default value: `false`

If set to true, will try to merge an existing `.gitignore` if it exists in `src` and append the ignored files in it.

## Contributing

If you can think of a way to unit test this plugin please take a shot at it.