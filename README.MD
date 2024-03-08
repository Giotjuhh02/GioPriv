## How to use?
- Open the database containing the entries that should be exported
- Create a folder `SubsetExportSettings` under the root folder
- For each export job (target database) create a new entry:

| Setting                                                   | Description                                                             | Optional                                   | Example                                 |
| --------------------------------------------------------- | ----------------------------------------------------------------------- | ------------------------------------------ | --------------------------------------- |
| `Title`                                                   | Name of the job                                                         | No                                         | `SubsetExport_MobilePhone`              |
| `Password`                                                | The password for the target database                                    | Yes, if `SubsetExport_KeyFilePath` is set  | `SecurePW!`                             |
| `Expires`                                                 | If the entry is expired the job is disabled and won't be executed       | `-`                                        | `-`                                     |
| `SubsetExport_KeyFilePath`<br>[string field]              | Path to a key file                                                      | Yes, if `Password` is set                  | `C:\keys\mobile.key`                    |
| `SubsetExport_TargetFilePath`<br>[string field]           | Path to the target database.<br>(Absolute, or relative to source database parent folder.) (`,` to delimit multiple target dbs) | No                       | `C:\sync\mobile.kdbx`<br>or<br>`mobile.kdbx`<br>or<br>`..\mobile.kdbx` |
| `SubsetExport_Tag`<br>[string field]                      | Tag(s) for filtering (`,` to delimit multiple tags - `,` is not allowed in tag names)| Yes, if `SubsetExport_Group` is set        | `MobileSync`                            |
| `SubsetExport_Group`<br>[string field]                    | Group(s) for filtering (`,` to delimit multiple groups - `,` is not allowed in group names)| Yes, if `SubsetExport_Tag` is set          | `MobileGroup`                           |
| `SubsetExport_KeyTransformationRounds`<br>[string field]  | Overwrite the number of KeyTransformationRounds for AesKdf              | Yes                                        | `10000000`                              |
| `SubsetExport_RootGroupName`<br>[string field]            | Overwrite the name of the root group in the target database             | Yes                                        | `NewRootGroupName`                      |
| `SubsetExport_FlatExport`<br>[string field]               | If `True` no groups will be created in the target database (flat export)| Yes (defaults to `False`)                  | `True`                                  |
| `SubsetExport_OverrideTargetDatabase`<br>[string field]   | If `True` the traget database will be overriden, otherwise the enries will added to the target database | Yes (defaults to `True`) | `True`                    |
| `SubsetExport_OverrideEntryOnlyNewer`<br>[string field]   | If `True` only newer entries will overrides older entries (`OverrideTargetDatabase` is `False`)| Yes (defaults to `False`) | `True`                             |
| `SubsetExport_OverrideEntireGroup`<br>[string field]      | If `True` will override entire group in target Database (`OverrideTargetDatabase` is `False`)| Yes (defaults to `False`) | `True`                             |
| `SubsetExport_Argon2ParamIterations`<br>[string field]    | Overwrite the number of iterations of Argon2Kdf                         | Yes                                        | `2`                                     |
| `SubsetExport_Argon2ParamMemory`<br>[string field]        | Overwrite the memory parameter of Argon2Kdf                             | Yes                                        | `1048576` = 1MB                         |
| `SubsetExport_Argon2ParamParallelism`<br>[string field]   | Overwrite the parallelism parameter of Argon2Kdf<br>Typical parallelism values should be less or equal than to two times the number of available processor cores (less if increasing does not result in a performance increase) | Yes | `2` |
| `SubsetExport_ExportUserAndPassOnly`<br>[string field]    | If `True` only the username and password will be exported to the target database. | Yes (defaults to `False`) | `True`                             |

- Every time the (source) database is saved the target databases will be recreated automatically
- To disable an export job temporarily just move its entry to another folder
- If both `SubsetExport_Tag` and `SubsetExport_Group` are set, only entries matching *both* will be exported

![create](https://user-images.githubusercontent.com/5115160/38439682-da51a266-39de-11e8-9cc4-744d5a3f0dae.png)

## KeePassSubsetExport vs Partial KeePass Database Export
I started developing KeePassSubsetExport before [Partial KeePass Database Export](https://github.com/heinrich-ulbricht/partial-keepass-database-export) was published, so the basic functionality is similar.  
But KeePassSubsetExport has some advantages:
- The folder structure is copied to the target database
- Multiple export jobs are supported
- Key-File protection of the target databases is supported
- KeyTransformationRounds of the target database is set to the number of the source database (can be overwritten)
- Exports can be limited to a folder (can be combined with a tag filter)
- Field references support (in the export job password field and the following entry fields: Title, Username, Url and Password)
