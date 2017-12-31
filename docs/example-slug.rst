Custom nameCallback + slug + windows problem
----------------

.. code:: php

  use Cake\Utility\Inflector;

.. code:: php

  $this->addBehavior('Josegonzalez/Upload.Upload', [
    'file' => [
        'filesystem' => [
            'root' => ROOT . DS . 'webroot' . DS . 'img' . DS // avoid backslash for windows
        ],
        'path' => '{field}{year}',
        'fields' => [
            'dir' => 'dir',
            'size' => 'size',
            'type' => 'type'
        ],
        'nameCallback' => function ($data, $settings) {
            $filename = pathinfo($data['name'], PATHINFO_FILENAME);
            $filename = Inflector::slug($filename, '-');
            $ext = pathinfo($data['name'], PATHINFO_EXTENSION);
            if (!empty($ext)) {
                $filename = $filename . '.' . $ext;
            }
            return date('dmYHis') . '-' . strtolower($filename);
        },
        'transformer' => 'Josegonzalez\Upload\File\Transformer\SlugTransformer',
        'deleteCallback' => function ($path, $entity, $field, $settings) {
            return [
                $path . DS . $entity->{$field}
            ];
        },
        'keepFilesOnDelete' => false
    ]
  ]);
