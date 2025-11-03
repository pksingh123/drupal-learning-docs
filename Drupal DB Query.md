#Initialise databaseConnect object
$database = \Drupal::database();
# Db Select: db select query used for fetch reacord from table
$database->select('tableName','tableAlias')->fields('tableAlias')->condition('id', $id)->execute()->fetchObject();

#Db Insert: db insert query used for insert records in specific table

$database->insert('tableName')->fields(['fieldNameOne','fieldNameTwo','fieldNamethree'])->value([$fieldOneValue, $fieldTwoValue, $fieldThreeValue])->execute();

#Db Update: db update query used to modify existing records in a database table based on specific condition

$databse->update('tableName')->fields(['fieldNmeOne'=>$fieldOneValue, 'fieldNameTwo'=>$fieldTwoValue])->condition('id', $id)->execute();

#Db Merge: db merge merge query is used when you want to either insert a new record if it doesn’t exist, or update an existing record if it does.

$database->merge('tableName')->key(['id' => $id ])->fields(['fieldNameOne'=>$fieldOneValue, 'fieldNameTwo'=>$fieldTwoValue])->execute();

#Db Delete: db delete query is used to delete a record from database table based on specific condition

$database->delete('tableName')->condition('id', $id)->execute();
