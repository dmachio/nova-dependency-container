<template>
	<div v-if="dependenciesSatisfied">
		<div v-for="childField in field.fields">
			<component
				:is="'detail-' + childField.component"
				:resource-id="resourceId"
				:resource-name="resourceName"
				:field="childField"
				:ref="'field-' + childField.attribute"
			/>
		</div>
	</div>
</template>

<script>
	export default {
		props: ['resource', 'resourceName', 'resourceId', 'field'],

		created() {
			// @note fix for inline-relationships resulting in incorrect attribute value
			this.field.attribute = this.field.attribute.replace(this.field.attrib, '');
			
			this.updateDependencyStatus()
		},

		data() {
			return {
				dependenciesSatisfied: false,
			}
		},

		methods: {

			updateDependencyStatus() {
				for (let dependency of this.field.dependencies) {
					if(dependency.satisfied) {
						this.dependenciesSatisfied = true;
						return;
					}
				}

				this.dependenciesSatisfied = false;
			},

		}
	}
</script>
