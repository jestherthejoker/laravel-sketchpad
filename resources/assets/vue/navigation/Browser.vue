<template>

	<div id="browser" class="controller-list">

		<!-- controllers -->
		<section id="controllers" class="col-xs-5">
			<ul class="nav nav-pills nav-stacked">
				<template v-for="folder in folders">
					<folder :route="folder.route"></folder>
					<controller
						v-for="controller in folder.controllers"
						:controller="controller">
					</controller>
				</template>
			</ul>
		</section>

		<!-- methods -->
		<section id="methods" class="col-xs-7">
			<ul class="nav nav-pills nav-stacked">
				<template v-for="result in methods">
					<group
						v-if="result.group"
						:group="result.group"
					></group>
					<method
						v-for="method in result.methods"
						:method="method">
					</method>
				</template>
			</ul>
		</section>

	</div>

</template>

<script>

// state
import store        from '../../js/state/store.js';
import state        from '../../js/state/state.js';
import settings     from '../../js/state/settings.js';

import Folder		from './components/Folder.vue';
import Controller	from './components/Controller.vue';
import Group		from './components/Group.vue';
import Method		from './components/Method.vue';

export default
{
	name:'Browser',

	components:
	{
		Folder,
		Controller,
		Group,
		Method,
	},

	computed:
	{
		folders ()
		{
			// variables
			let results = [];
			let result
			let lastController

			// loop
			if (store && store.controllers) {
				store.controllers
					.forEach((controller, index) => {
						if(index === 0 || controller.folder !== lastController.folder) {
							result = {route:controller.folder, controllers:[]}
							results.push(result)
						}
						result.controllers.push(controller)
						lastController = controller
					});
			}

			// return
			return results;
		},

	    methods ()
	    {
			// variables
			let results = [];
			let result

			// loop
			if (state && state.controller) {
				state.controller.methods
					.forEach((method, index) => {
						if(index === 0 || method.tags.group) {
							result = {group:method.tags.group, methods:[]}
							results.push(result)
						}
						if(method.name === 'index') {
							return
						}
						if(!settings.ui.showArchived && method.tags.archived) {
							return
						}
						result.methods.push(method)
					});
			}

			// return
			return results;
	    }

	}

}

</script>

<style lang="scss">
	
</style>