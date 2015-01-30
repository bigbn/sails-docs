# myApp/Gruntfile.js
### Назначение

Sails использует [Grunt](http://gruntjs.com) для управления файлами ресурсов. Этот файл содержит информацию для задач GRUNT-а которые Sails использует для своих нужд.

Интеграция Sails с Grunt полностью настраиваемая, но в большинстве случаев этот файл лучше не трогать. Вместо этого, используйте `myApp/tasks/` для задания своей произвольной логики.


### Дополнительная информация
> Дополнительная информации о том как использовать Grunt для работы со статическими файлами ресурсов: http://gruntjs.com/configuring-tasks


<docmeta name="uniqueID" value="Gruntfilejs375150">
<docmeta name="displayName" value="Gruntfile.js">

<div www-sjs-anatomy-boilerplate>

```javascript
/**
 * Gruntfile
 *
 * Этот сценарий запускается когда вы запускаете команду `grunt` или `sails lift`.
 * Его назначение - загрузка задач Grunt-а в каталог `tasks` вашего проекта
 * чтобы вы могли добавлять или удалять задачи как вы считаете нужным.
 * Для дополнительной информации о том как это работает, смотрите файл `README.md` 
 * сгенерированный в вашей папке `tasks`
 *
 * ВНИМАНИЕ:
 * Если вы не знаете, что вы делаете, вы не должны изменять этот файл
 * Вместо этого, лучше ознакомьтесь с каталогом `tasks`.
 */

module.exports = function(grunt) {


	// Load the include-all library in order to require all of our grunt
	// configurations and task registrations dynamically.
	var includeAll;
	try {
		includeAll = require('include-all');
	} catch (e0) {
		try {
			includeAll = require('sails/node_modules/include-all');
		}
		catch(e1) {
			console.error('Could not find `include-all` module.');
			console.error('Skipping grunt tasks...');
			console.error('To fix this, please run:');
			console.error('npm install include-all --save`');
			console.error();

			grunt.registerTask('default', []);
			return;
		}
	}


	/**
	 * Loads Grunt configuration modules from the specified
	 * relative path. These modules should export a function
	 * that, when run, should either load/configure or register
	 * a Grunt task.
	 */
	function loadTasks(relPath) {
		return includeAll({
			dirname: require('path').resolve(__dirname, relPath),
			filter: /(.+)\.js$/
		}) || {};
	}

	/**
	 * Invokes the function from a Grunt configuration module with
	 * a single argument - the `grunt` object.
	 */
	function invokeConfigFn(tasks) {
		for (var taskName in tasks) {
			if (tasks.hasOwnProperty(taskName)) {
				tasks[taskName](grunt);
			}
		}
	}




	// Load task functions
	var taskConfigurations = loadTasks('./tasks/config'),
		registerDefinitions = loadTasks('./tasks/register');

	// (ensure that a default task exists)
	if (!registerDefinitions.default) {
		registerDefinitions.default = function (grunt) { grunt.registerTask('default', []); };
	}

	// Run task functions to configure Grunt.
	invokeConfigFn(taskConfigurations);
	invokeConfigFn(registerDefinitions);

};

```
