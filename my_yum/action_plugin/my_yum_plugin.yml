# File: my_yum_plugin.py
from ansible.plugins.action import ActionBase
from ansible.module_utils.six.moves import StringIO
from ansible.module_utils._text import to_bytes

class ActionModule(ActionBase):
    def run(self, tmp=None, task_vars=None):
        # Retrieve task variables
        pkg_name = self._task.args.get('name')

        # Check if the package is installed
        command = "rpm -q {0}".format(pkg_name)
        rc, out, err = self._connection.exec_command(command)
        stdout = to_bytes(out.read())
        if rc == 0:
            return {'changed': False}

        # Install the package
        command = "yum install -y {0}".format(pkg_name)
        rc, out, err = self._connection.exec_command(command)
        stdout = to_bytes(out.read())
        if rc != 0:
            self._display.display("Failed to install {0} package".format(pkg_name))
            return {'failed': True}

        return {'changed': True}
