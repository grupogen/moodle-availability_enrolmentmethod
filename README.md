moodle-availability_enrolmentmethod
========================

Moodle availability plugin which lets users restrict resources, activities and sections based on enrolment methods


Requirements
------------

This plugin requires Moodle 3.11+

Installation
------------

Install the plugin like any other plugin to folder
/availability/condition/enrolmentmethod

See http://docs.moodle.org/en/Installing_plugins for details on installing Moodle plugins


Usage
----------------

After installing the plugin, it is ready to use without the need for any configuration.

Teachers (and other users with editing rights) can add the "Enrolment method" availability condition to activities / resources / sections in their courses. While adding the condition, they have to define the enrolment method which students have to have in course context to access the activity / resource / section.

If you want to learn more about using availability plugins in Moodle, please see https://docs.moodle.org/en/Restrict_access.

How this plugin works
--------------------------------

The availability plugin checks if the user has the given enrolment method and, if yes, grants access to the restricted activity.

However, there is the capability moodle/course:viewhiddenactivities (see https://docs.moodle.org/en/Capabilities/moodle/course:viewhiddenactivities) which is contained in the manager, teacher and non-editing teacher roles by default. If a user has a role which contains moodle/course:viewhiddenactivities, he is able to use an activity / resource / section even if the teacher has restricted it with availability_enrolmentmethod to some other role.

Because of that, availability_enrolmentmethod can't be used to hide activities / resources / sections from users who already are allowed to view hidden activities in the course. Use this availability restriction plugin wisely and explain to your teachers what is possible and what is not.


## License

Licensed under the [GNU GPL License](http://www.gnu.org/copyleft/gpl.html).

## Fork GEN

Forkeado para
- Adicionar o composer.json

- Adicinar tradução dos termos pt_br

- Criar texto de descrição específico para os métodos de inscrição: manual e magento
```
// Enrolment methods used to grant access to purchased content (manual and Magento)
// get a dedicated message instead of exposing the technical enrolment method name.
$purchaseenroltypes = ['manual', 'magento'];
$instances = $manager->get_enrolment_instances(true);
$enroltype = $instances[$this->enrolmentmethodid]->enrol ?? null;
if (!$not && in_array($enroltype, $purchaseenroltypes, true)) {
    return get_string('requires_purchase', 'availability_enrolmentmethod');
}
```

- Criada a função is_userenrolment_active para verificar se o usuário está inscrito com o método de inscrição e se a inscrição nesse método está ativa
```
/**
     * Whether a user_enrolments record counts as currently active: status active and within
     * its timestart/timeend window (mirrors is_enrolled(..., onlyactive: true) in enrollib.php).
     *
     * @param stdClass $userenrolment Record from the user_enrolments table
     * @return bool
     */
    private function is_userenrolment_active(stdClass $userenrolment): bool {
        $now = time();
        return (int) $userenrolment->status === ENROL_USER_ACTIVE
                && (int) $userenrolment->timestart < $now
                && ((int) $userenrolment->timeend === 0 || (int) $userenrolment->timeend > $now);
    }
```
Utilizada nas funções:
```
filter_user_list() e is_available()
```