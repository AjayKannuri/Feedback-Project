





DataSchemaModel dataSchemaModel = approvalMatMeta.getDataSchema();
Set<PropertyModel> propertyModel = dataSchemaModel.getPropertyData();
List<RolesModel> rolesModel = dataSchemaModel.getRolesData();
List<StagesModel> stagesModel = dataSchemaModel.getStagesData();
Property property=new Property();


ApprovalMatrixmeta approvalMatrixmeta = new ApprovalMatrixmeta();

// new entities...........

Set<Property> propertySet = new HashSet<Property>();
List<Roles> rolesList = new ArrayList<Roles>();
List<Stages> stagesList = new ArrayList<Stages>();

DataSchema dataSchema = new DataSchema();

// ...................


Iterator<PropertyModel> i = propertyModel.iterator(); 
while (i.hasNext()) 
{
		PropertyModel propertyModel = i.next();
		Property property = new Property();
		property.setName(propertyModel.getName());
		property.setType(propertyMode.getType());
		property = propertyRepository.save(property);
		propertySet.add(property);
}

for(RolesModel rolesModel : rolesModel) {

	Roles roles = new Roles();
	roles.setRoleName(rolesModel.getRoleName());
	roles = rolesRepository.save(roles);
	rolesList.add(roles);
	
}

for(StagesModel stage : stagesModel) {
	Stages stages = new Stages();
	stages.setStageName(stage.getStageName());
	stages = stagesRepository.save(stages);
	stagesList.add(stages);
}

dataSchema.setPropertyData(propertySet);
dataSchema.setRolesData(rolesList);
dataSchema.setStagesData(stagesList);

dataSchema = dataSchemaRepository.save(dataSchema);

approvalMatrixmeta.setApplication(approvalMatMeta.getApplication());
approvalMatrixmeta.setCompany(approvalMatMeta.getCompany());
approvalMatrixmeta.setEntity(approvalMatMeta.getEntity());
approvalMatrixmeta.setDataSchema(dataSchema);
approvalMatrixmeta = approvalMatrixmetaRepository.save(approvalMatrixmeta);


// resaving the primary keys....

dataSchema.setApprovalMatrixMeta(approvalMatrixmeta);
dataSchema = dataSchemaRepository.save(dataSchema);


Iterator<Property> i = dataSchema.getPropertyData().iterator(); 
while (i.hasNext()) 
{
		Property property = i.next();
		property.setDataSchema(dataSchema);
		propertyRepository.save(property);
}

for(Roles roles : dataSchema.getRolesData()) {

	roles.setDataSchema(dataSchema);
	roles = rolesRepository.save(roles);
	
}

for(Stages stage : dataSchema.getStagesData()) {
	stage.setDataSchema(dataSchema);
	stages = stagesRepository.save(stage);
}

// resaved the primary keys..................






